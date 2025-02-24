---
title: Running Xcode tests
redirect_from: []
date: '2019-02-22T15:33:24.000+00:00'
tag:
- testing
- ios
description: 'Run Xcode tests on Bitrise by using our Xcode Test for iOS Step. Configure
  the tests in our Workflow Editor with the same options as in Xcode, and generate
  code coverage files easily. '
summary: ''
menu:
  testing-main:
    weight: 6

---
To run Xcode tests on Bitrise, you need to have test targets defined in your projects. You need two Steps to run Xcode tests and view their results:

* **Xcode Test for iOS**.
* **Deploy to** [**Bitrise.io**](http://Bitrise.io).

{% include message_box.html type="note" title="Code signing files" content="Running Xcode tests and deploying their results to Bitrise do not require any code signing files. So don't worry about them just yet!"%}

The **Xcode Test for iOS** step runs the pre-defined Xcode tests. It has a default configuration that will work if the tests are written correctly. You can find the same configuration options in Xcode, too.

You can also generate code coverage files, and export the test results as a compressed zip file.

{% include message_box.html type="info" title="Test reports" content="Your Xcode test results will be exported to Test Reports: check out our [Test Reports](/testing/test-reports/) guide to find out how to make it work for you."%}

## Configuring the Xcode tests on Bitrise

The default input values of the **Xcode Test for iOS** Step could work if your tests are written correctly. However, we recommend making sure that all the inputs have the value you want so the Step can do what you need!

{% include message_box.html type="warning" title="Exporting the generated result" content="The **Xcode Test for iOS** Step generates an .xcresult bundle and exports it to a location set in the Env Var `$BITRISE_XCRESULT_PATH`.

Be aware that some tools, like xcov, will look for the results bundle in its default location, not at  `$BITRISE_XCRESULT_PATH`. Because of this your build will fail when using those tools. You need to configure them to look for the bundle in the location set by the Env Var. Here’s an example xcov setup:

    xcov.report(
      scheme: 'ExampleScheme',
      workspace: 'example.xcworkspace',
      xccov_file_direct_path: ENV['BITRISE_XCRESULT_PATH'],
      include_targets: 'Example.app,ExampleShared.framework,ExampleWidget.appex'
    )

"%}

Check the following required inputs of the **Xcode Test for iOS** Step before running a build. These inputs determine the sort of tests the Step will run.

* **Scheme name**: the scheme you use must be marked as Shared in Xcode!
* **Device**: set it to the value that is shown in Xcode's device selection dropdown menu.
* **OS version**: set it to the value that is shown in Xcode's device selection dropdown menu.
* **Platform**: set it to the value that is shown in Xcode's device selection dropdown menu.

{% include message_box.html type="info" title="Headless mode" content="From Xcode 9 onwards, tests are run in headless mode by default: this means that the simulator will run in the background only. To change it, go to the Step's Debug input group and set the **Run the test in headless mode?** input's value to no. However, with this option, tests will take more time."%}

{% include message_box.html type="warning" title="Using xcpretty" content="The xcpretty output tool does not support parallel tests. If parallel tests are enabled in your project, go to the Step's Debug input group and set the **Output tool** input's value to xcodebuild."%}

## Rerunning failed tests (Not available in Xcode 13+)

The **Should retry test on failure? (Not available in Xcode 13+)** input in the **Debug** section of the **Xcode Test for iOS** Step allows you to automatically rerun ALL your tests, not just the failed ones. If you set this input to `yes`, the Step will run `xcodebuild` one more time in the case of test failure.

From Xcode 13 and above the **Should retry test on failure? (Not available in Xcode 13+)** feature is not available anymore. We recommend you use **Test Repetitions Mode (Available in Xcode 13+)** input with the `retry_on_failure` option selected. This allows you to rerun only the failed test/s instead of running all your tests. You can find this test repetition feature with our **Xcode Test for iOS** Step from version 3.0.0 and above.

## Test Repetitions

[Xcode’s test repetition modes](https://developer.apple.com/videos/play/wwdc2021/10296/) are available with the [Xcode Test for iOS Step](https://www.bitrise.io/integrations/steps/xcode-test) on stacks running Xcode 13 and above on Bitrise. With test repetitions, you can run any type of tests multiple times in various ways such as retry on failure, until failure and until max repetitions.

The **Xcode Test for iOS** Step’s **Test Repetitions Mode (Available in Xcode 13+)** input offers the following options:

* `none`: The tests won’t repeat.
* `until_failure`: Repeats a test until the test fails or until the maximum repetition. The default number of test runs is three.
* `retry_on_failure`: Failed tests run until they succeed or until the repetition number you specify. The default number of test repetitions is three.
* `up_until_maximum_repetitions`: Reruns all tests until maximum test repetition is reached regardless of the test outcomes.

Please note that these options are only available from **Xcode Test for iOS** version 3.0.0 and above.

With the **Maximum Test Repetitions (Available in Xcode 13+)** input you can specify the maximum number of test repetitions. Please note that you have to add a greater number than one if the **Test Repetition Mode (Available in Xcode 13+)** input is set to other than `none`.

Enable the **Relaunch Tests for Each Repetition (Available in Xcode 13+)** input to launch tests in a completely new process for each repetition.

<div class="banner"><img src="/assets/images/banner-bg-888x170.png" style="border: none;"> <div class="deploy-text">Let's run Xcode tests on Bitrise</div> <a target="_blank" href="https://app.bitrise.io/dashboard/builds"><button class="button">Go to your app</button></a></div>