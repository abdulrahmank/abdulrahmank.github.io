Being able to create a testable code is often a challenge when it comes to mobile development. Flutter is also not an exception, stick along to understand how to develop flutter application with a testable and clean architecture.

**MVP pattern - Overview:**

When I was working actively on Android app development, we initially followed Model-View-Presenter (MVP) pattern. In MVP pattern we have all the business logic in the Presenter class which is a basic Java/Kotlin class which can be tested easily as it doesn't have any Android component class involved. Model class acts as POJO and Data access layer. View is nothing but the class which involves android components and creates the UI. The View class will not have any logic in it, but will be calling the methods in Presenter class to render the UI. Since Presenter code has extensive unit tests, and Since View class is calling Presenter class for any logic to render, we would often not do unit testing for the View class.

**MVVM pattern - Overview:**

When ViewModel and Room was introduced, we were planning to move away from MVP to (Model-View-ViewModel) MVVM pattern. In MVVM, Model and View are the same as MVP, instead of Presenter class we will have ViewModel class. This ViewModel class is the normal class that doesn't have android ui components, and will be easily testable. One key difference between Presenter and View Model is that, View Model will drive the UI changes and will push the data to the view, in contrast to Presenter where View will drive the UI changes and pulls the data from presenter. With Room, ViewModel will listen to db changes and update the UI by notifying it.

When I started Flutter app development and started looking for the ecosystem on which pattern to follow, I settled with MVVM using Isar database and Provider class.

Isar database gives you a very performant alternative to hive db and also gives us the ability to query the database. The query interface is very matured that I have migrated an existing project that was using sqllite with lots of queries with Isar, and started using Isar database for the new app. Shoutout to Simon Leier for creating such a wonderful DB.

Provider package gives us ability to extend ChangeNotifier and drive UI changes by notifying there is a change.

Enough of explanation look at the following code:
```
// View - just renders the UI
class DemoPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text('Demo Page'),
        ),
        body: ChangeNotifierProvider<DemoPageViewModel>(
          create: (BuildContext context) => DemoPageViewModel(),
          child: Consumer<DemoPageViewModel>(builder: (context, model, child) {
            return Column(
              children: [
                Text(model.getVersionNumber().toString()),
                TextButton(
                  child: Text("Add"),
                  onPressed: () {
                    // User action to increment version
                    model.increment();
                  },
                ),
              ],
            );
          }),
        ));
  }
}

// ViewModel - Handles the State management and drives the UI changes
class DemoPageViewModel extends ChangeNotifier {
  late Version version;
  Stream<List<Version>>? versionDBChangeStream;

  DemoPageViewModel() {
    // final isar = await Isar.open([EmailSchema]); -- should be called when starting the app in main.dart
    version = IsarInitializer.isar.version.filter().sortById().findFirstSync();
    versionDBChangeStream =
        sarInitializer.isar.version.filter().build().watch();
    versionDBChangeStream?.listen((newVesions) {
      version = newVesions.first;
      notifyListeners();
    });
  }

  int getVersionNumber() {
    return version.versionNumber ?? 0;
  }

  void increment() {
    IsarInitializer.isar.writeTxnSync(() =>
        IsarInitializer.isar.version.putSync(Version(getVersionNumber() + 1)));
  }
}

// Model - acts as the data access layer 
@collection
class Version {
  Id id = Isar.autoIncrement;
  int? versionNumber;

  Version(this.versionNumber);
}
```

The DemoPage just renders the UI, the state mangement, notifying the UI of the change is taken care by ViewModel class which in turn uses Isar to achieve it. On a user action, the db is changed and hence the Viewmodel which is listening to db is notified, which inturns notifies the UI.

The ViewModel, which doesn't contain UI rendering logic, can be easily tested. I will Be sharing the examples of unit testing the View model in the next Artcile.

This article shows how to implement MVVM in flutter. Feel free to share your thoughts and different approach that you have followed.