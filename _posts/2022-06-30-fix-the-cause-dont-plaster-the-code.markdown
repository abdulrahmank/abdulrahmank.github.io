When there is a bug, what we tend to do is, fix the bug in the easiest way possible and deploy the fix.

This is in a way required to improve the reliability, availability of the system and to improve the MTTR (Mean Time To Recover). As a follow up step, what we need to make sure is why this bug occurred in the first place, and can that affect other functionalities of the product.

The same is applicable for a change request as well, instead of making lots of code changes to adapt the code change, we can instead fix the source and do a step of back filling migration to fix the existing data as well.

For Instance:

In a library management system, if we have a change requirement to search books by their name without case sensitivity, then we can take 2 approaches:

1. To change all the places where we search the book by it's name like this:

```Book.find_by(name: book_name) # From this
Book.where("lower(name) = ?", book_name) # To this```

1. Or To change the source where the book name is entered:

```Book.create(name: book_name.downcase)```

And running a back filling task/migration to update the book name:

```Book.all.each { |book| book.update(name: book.name.downcase) } # Not optimised, but trying to convey the idea```

In the 2nd approach, we tend to fix the data and existing code complexity doesn't increase. Also the code changes reduces thus reducing the risk of leaving any code which actually needs to be changed.

To conclude its always better to go all the way up and fix the source instead of the places of usage.
