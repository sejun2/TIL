Function that executes your background work.
You should return whether the task ran successfully or not.
[taskName] Returns the value you provided when registering the task.
iOS will always return [Workmanager.iOSBackgroundTask]    
`Workmanager.IOSBackgroundTask ==>  static const String iOSBackgroundTask = "iOSPerformFetch";`

```
typedef BackgroundTaskHandler = Future<bool> Function(
    String taskName,
    Map<String, dynamic>? inputData
    );
   
  ```
  
AOS, IOS 두 플랫폼에서 동작하는 함수
```
void executeTask(final BackgroundTaskHandler backgroundTask) {
    WidgetsFlutterBinding.ensureInitialized();
    _backgroundChannel.setMethodCallHandler((call) async {
      final inputData = call.arguments["be.tramckrijte.workmanager.INPUT_DATA"];
      return backgroundTask(
        call.arguments["be.tramckrijte.workmanager.DART_TASK"],
        inputData == null ? null : jsonDecode(inputData),
      );
    });
    _backgroundChannel.invokeMethod("backgroundChannelInitialized");
  }
```

AOS 에서 Wokrmanager을 이용하는 함수
```
 /// This call is required if you wish to use the [WorkManager] plugin.
  /// [callbackDispatcher] is a top level function which will be invoked by Android
  /// [isInDebugMode] true will post debug notifications with information about when a task should have run
  Future<void> initialize(
    final Function callbackDispatcher, {
    final bool isInDebugMode = false,
  }) async {
    Workmanager._isInDebugMode = isInDebugMode;
    final callback = PluginUtilities.getCallbackHandle(callbackDispatcher);
    assert(callback != null,
        "The callbackDispatcher needs to be either a static function or a top level function to be accessible as a Flutter entry point.");
    if (callback != null) {
      final int handle = callback.toRawHandle();
      await _foregroundChannel.invokeMethod<void>(
        'initialize',
        JsonMapperHelper.toInitializeMethodArgument(
          isInDebugMode: _isInDebugMode,
          callbackHandle: handle,
        ),
      );
    }
  }

  /// Schedule a one off task
  /// A [uniqueName] is required so only one task can be registered.
  /// The [taskName] is the value that will be returned in the [BackgroundTaskHandler]
  /// The [inputData] is the input data for task. Valid value types are: int, bool, double, String and their list
  Future<void> registerOneOffTask(
    final String uniqueName,
    final String taskName, {
    final String? tag,
    final ExistingWorkPolicy? existingWorkPolicy,
    final Duration initialDelay = _noDuration,
    final Constraints? constraints,
    final BackoffPolicy? backoffPolicy,
    final Duration backoffPolicyDelay = _noDuration,
    final Map<String, dynamic>? inputData,
  }) async =>
      await _foregroundChannel.invokeMethod(
        "registerOneOffTask",
        JsonMapperHelper.toRegisterMethodArgument(
          isInDebugMode: _isInDebugMode,
          uniqueName: uniqueName,
          taskName: taskName,
          tag: tag,
          existingWorkPolicy: existingWorkPolicy,
          initialDelay: initialDelay,
          constraints: constraints,
          backoffPolicy: backoffPolicy,
          backoffPolicyDelay: backoffPolicyDelay,
          inputData: inputData,
        ),
      );

  /// Schedules a periodic task that will run every provided [frequency].
  /// A [uniqueName] is required so only one task can be registered.
  /// The [taskName] is the value that will be returned in the [BackgroundTaskHandler]
  /// a [frequency] is not required and will be defaulted to 15 minutes if not provided.
  /// a [frequency] has a minimum of 15 min. Android will automatically change your frequency to 15 min if you have configured a lower frequency.
  /// The [inputData] is the input data for task. Valid value types are: int, bool, double, String and their list
  Future<void> registerPeriodicTask(
    final String uniqueName,
    final String taskName, {
    final Duration? frequency,
    final String? tag,
    final ExistingWorkPolicy? existingWorkPolicy,
    final Duration initialDelay = _noDuration,
    final Constraints? constraints,
    final BackoffPolicy? backoffPolicy,
    final Duration backoffPolicyDelay = _noDuration,
    final Map<String, dynamic>? inputData,
  }) async =>
      await _foregroundChannel.invokeMethod(
        "registerPeriodicTask",
        JsonMapperHelper.toRegisterMethodArgument(
          isInDebugMode: _isInDebugMode,
          uniqueName: uniqueName,
          taskName: taskName,
          frequency: frequency,
          tag: tag,
          existingWorkPolicy: existingWorkPolicy,
          initialDelay: initialDelay,
          constraints: constraints,
          backoffPolicy: backoffPolicy,
          backoffPolicyDelay: backoffPolicyDelay,
          inputData: inputData,
        ),
      );
```
작업 취소 함수
```
  /// Cancels a task by its [uniqueName]
  Future<void> cancelByUniqueName(final String uniqueName) async =>
      await _foregroundChannel.invokeMethod(
        "cancelTaskByUniqueName",
        {"uniqueName": uniqueName},
      );

  /// Cancels a task by its [tag]
  Future<void> cancelByTag(final String tag) async =>
      await _foregroundChannel.invokeMethod(
        "cancelTaskByTag",
        {"tag": tag},
      );

  /// Cancels all tasks
  Future<void> cancelAll() async =>
      await _foregroundChannel.invokeMethod("cancelAllTasks");
```
