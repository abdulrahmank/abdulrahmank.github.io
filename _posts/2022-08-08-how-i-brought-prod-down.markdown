

What as a software developer we are afraid of, has happened. I deployed my changes in a service (we had micro-services architecture), and started getting Airbrake alerts, PagerDuty on the service that I just deployed. All the errors said request timeout, guess what? I have brought down the service in production environment.

To give little context about the criticality of the service, it is the only service responsible for giving the ability for the users to pay us (Yes you heard it correct, payment-service). We had separate db for the payment-service, which had around 2 million user records. We had at-least 200 requests per second.

P.S. I'm not very sure regarding the numbers, but this is the approximation.

The service was written in RubyOnRails.

When we reviewed the last changes, it was unit tested, and very simple change which can't be suspected for bringing down the service. We added a code, to create an endpoint in which we needed to join two tables and query the result for a single user. So the code was something like this:

```
# routes.rb
resources :users do
  post 'pay'
end

-------------------------------------------------------------------------

class PaymentController
#...other methods
def pay
  User.eager_loads(:payment_details).where(id: params[:id])

  # few other lines of code to complete the functionality
end
```

The code at the initial sight looks fine, the unit test was something like this:

```
before do
  @user = create(:user)
  @payment_detail = create(:payment_details, user: user)
end

it 'Should get payment detail of the user' do
  post '/pay', params: { id: @user.id }
  
  expect(response).to eq(@payment_detail)
end
```

The problem with unit test was that, I have provided the `id` in the params, I didn't take into account how rails will pass the user_id sent from the request to the controller. Actually instead of `params[:id]`, we should have used `params[:user_id]`.

The result was that we were loading all the 2M records in the memory for each request, because of which the instance crashed after 2/3 requests. And all other requests kept on waiting and timed out with 5XX error.

Since we had a good CD pipeline, we were quickly able to rollback the current changes, so that the system will be healthy once again. And then we changed the line of code to this:
```
  User.eager_loads(:payment_details).where(id: params[:user_id])
```
**Conclusion:**

    1. In Rails when adding an endpoint in routes, always verify the syntax using `rake routes`.
    1. Always test each and every line of code that goes into production, even if unit tested, at least do an end to end manual test once for happy path. Only unit testting is not always enough.
    1. Have a good CD pipeline to have less MTTR (Mean Time To Recovery)
    1. Have monitoring tools to convey regarding system performance
    1. Don't deploy and leave, be responsible to monitor for sometime if things are fine, and act accordingly.

