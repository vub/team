# GitLab runner
## Install on macOS

In the future there will be a brew package, but for now you have to manually download the macOS binary.

### Installation

Download the binary for your system:

```
$ sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-ci-multi-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-ci-multi-runner-darwin-amd64
```

You can download a binary for every available version as described in Bleeding Edge - download any other tagged release.

Give it permissions to execute:

```
$ sudo chmod +x /usr/local/bin/gitlab-runner
```

Start the service:

```
$ sudo gitlab-runner start
```

Stop the service:

```
$ sudo gitlab-runner stop
```

## Setting up your Xcode project

Creating a new iOS project.

![create-project](https://about.gitlab.com/images/blogimages/setting-up-gitlab-for-ios-projects/1_create-new-xcode-project.png)

Enabling Include Unit Tests and Include UI Tests options for the project. Xcode will create a template test class with some sample tests, which we'll use in this post as the test suite that GitLab CI runs to verify a build.

![setup-testing-options](https://about.gitlab.com/images/blogimages/setting-up-gitlab-for-ios-projects/2_enable-unit-tests.png)

Choose where you'll save your iOS project. If you like, let Xcode create the git repository on your Mac.

![create-git](https://about.gitlab.com/images/blogimages/setting-up-gitlab-for-ios-projects/3_create-git-repository.png)

Once Xcode has created and opened your iOS project, you need to share its scheme. By sharing your scheme, GitLab CI gets context it needs to build and test your project. To share a scheme in Xcode, choose **Product > Scheme > Manage Schemes**.

![share-scheme](https://about.gitlab.com/images/blogimages/setting-up-gitlab-for-ios-projects/4_share-xcode-scheme.png)

Your Xcode project has been created with two test files; one includes sample unit tests, and the other includes sample UI tests. You can run Product > Test to run these tests, which will build your project, launch the Simulator, install the project on the Simulator device, and run the test suite. You can see the results right in Xcode:

![test-suite](https://about.gitlab.com/images/blogimages/setting-up-gitlab-for-ios-projects/5_test-suite-success-in-xcode.png)

The green checkmarks next to the test functions (both in the file, and in the Test navigator) show that all tests passed. We won't be referring to the Xcode project anymore, so if you like, you can close it.

The final step here is to install `xcpretty`. When Xcode builds and tests your project, `xcpretty` will transform the output into something more readable for you.

## Installing and registering the GitLab Runner

Registering runner

```
$ sudo gitlab-ci-multi-runner register

WARNING: Running in user-mode.
WARNING: The user-mode requires you to manually start builds processing:
WARNING: $ gitlab-runner run
WARNING: Use sudo for system-mode:
WARNING: $ sudo gitlab-runner...
```

Entering gitlab-ci URL
```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/ci):
https://gitlab.com/ci
```

#### The CI token for your project is available on GitLab's Project Settings page, under Advanced Settings. Each project has a unique token.

```
Please enter the gitlab-ci token for this runner:
<CI runner token from Project > Settings > Runner>
```

Enter runner's name. The register process suggests the name of your Mac as a description for the runner.
```
Please enter the gitlab-ci description for this runner:
[Your-Mac's-Name.local]:
```

Enter whatever tags you'd like to further identify this particular runner. This way, we can filter our build stages in GitLab by toolchain, platform, etc.

```
Please enter the gitlab-ci tags for this runner (comma separated):
ios_9-2, xcode_7-2, osx_10-11
```

The GitLab Runner has to run `xcodebuild` to build and test the project, so we select `shell` as the executor:

```
Please enter the executor: virtualbox, ssh, shell, parallels, docker, docker-ssh:
shell
Runner registered successfully. Feel free to start it, but if it's running
already the config should be automatically reloaded!
```

Continue with the rest of the Runner installation instructions (install and start), per the documentation.

Go to the Runners page in your Project Settings and voilà:

![runner-connected](https://storage.jumpshare.com/preview/RtyDVVkahcEMjkT6yhKz989KNlfv-VVazUJJKeFyCwkh-xIDAuho_zjbBlAirxktbvot-u8ETCeXYH0IboGGH90Iq-_ZMIwlJNqsu6s4bO0F1kR3dMUjedqC16uBUu85)

You can verify this by running

```
$ sudo gitlab-ci-multi-runner verify
WARNING: Running in user-mode.
WARNING: The user-mode requires you to manually start builds processing:
WARNING: $ gitlab-runner run
WARNING: Use sudo for system-mode:
WARNING: $ sudo gitlab-runner...

Veryfing runner... is alive                         runner=25c780b3
```

#### Configuring the build and test settings. To do so, open your text editor and enter the following:


  ```
  stages:
    - build

  build_project:
    stage: build
    script:
      - xcodebuild clean -project ProjectName.xcodeproj -scheme SchemeName | xcpretty
      - xcodebuild test -project ProjectName.xcodeproj -scheme SchemeName -destination 'platform=iOS Simulator,name=iPhone 6s,OS=9.2' | xcpretty -s
    tags:
      - ios_9-2
      - xcode_7-2
      - osx_10-11
  ```

 Save this file in your Xcode project folder as `.gitlab-ci.yml`.

After all, you can push your commit normally and you can check `Pipelines` tab to see CI build's log

## Notes
Let's go through the file with some details:

- The file first describes the stages available to each job. For simplicity, we have one stage (build) and one job (build_project).
- The file then provides the settings for each job. The build_project job runs two scripts: one to clean the Xcode project, and then another to build and test it. You can probably skip the cleaning script to save time, unless you want to be sure that you're building from a clean state.
- Under tags, add the tags you created when you registered the GitLab Runner.

There are also some things to look out for:

- Make sure to replace all references to ProjectName with the name of your Xcode project; if you're using a different scheme than the default, then make sure you pass in the proper SchemeName too (the default is the same as the ProjectName).
- In the xcodebuild test command, notice the -destination option is set to launch an iPhone 6S image running iOS 9.2 in the Simulator; if you want to run a different device (iPad, for example), you'll need to change this.
- If you're using a workspace rather than a project (e.g., because your app uses Cocoapods), change the -project ProjectName.xcodeproj options to -workspace WorkspaceName.xcworkspace. There are several options available to customize your build; run xcodebuild --help in the Terminal to explore these further.
