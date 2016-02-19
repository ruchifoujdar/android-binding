# Introduction #

The basic unit for Android application development is the Activity. However, the way that Android SDK design is overloading the Activity quite heavily. It is the place to control the flow of application, handling User input, communicate with data-layer, service-layer etc.

Android-Binding is a [MVVM](http://en.wikipedia.org/wiki/Model_View_ViewModel) (Model-View-ViewModel) framework, that helps freeing the Activity from working directly to User Interfaces. As an Activity, it's job is only to supply the ViewModel that the View requires to render the result while ViewModel is a class that with zero direct coupling with the View (and actually you might supply a different View to it).

By decoupling View (User Interface) logic and interaction from ViewModel, it also helps in Unit-Testing your application logic.

# Key Features #

  * Declare view binding in Layout XML files. No additional files needed.
  * Helps implements MVVM
  * Much easier for Unit-Testing
  * Model Validation that validates against ViewModel
  * Support Cursor results, and you can even validate the cursor result!