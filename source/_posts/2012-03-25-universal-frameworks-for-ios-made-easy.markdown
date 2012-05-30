---
layout: post
title: "Universal Frameworks for iOS made easy!"
date: 2012-03-25 15:58
comments: true
categories: iOS, ruby
author: Franklin Webber
---

On a recent iOS project we wanted to integrate the 
[Facebook iOS SDK](https://github.com/facebook/facebook-ios-sdk). The
integration went smoothy. We copied in the compiled library and the required
headers and were able to focus on the login, authentication flow.

## Problem

However, we later ran into trouble with the library working on the device but
not in the simulator. We would fix it for the tests and the simulator but find
out we had broken it again for the device. This back and forth continued until we had found out about Universal Frameworks.

## Narrative

We needed to compile the library in a format that would work on both on the 
iPhone and the simulator. Google lead us to this [invaluable post](http://db-in.com/blog/2011/07/universal-framework-iphone-ios-2-0/) 
that outlined the arduous path of creating a universal framework. A journey
indeed filled with peril as there are so many opportunities along the way to 
take a misstep and lose your way.

To the author's credit, he does a good job of outlining the necessary steps to
succeed. It is the shear overwhelming number of Xcode's [build settings](https://developer.apple.com/library/mac/#documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html) 
that can get in the way with crossing the finish line.

As the project continued we came again to a similar crossroads as we needed to
include Twitter authentication. We chose to use the 
[MPOAuth](https://github.com/thekarladam/MPOAuth) 
library but again found the need to generate a universal framework to save us
from the same particular annoyance (and others) that arose when it came time to
integrate the library. We followed through the same treacherous steps.

Having to do this twice has taught me that I do not want to do it again. There
is simply too many steps to get right. It was something that needed to be easier
and involve a lot less mouse-driven-development.

## Solution

[Xcoder](https://github.com/rayh/xcoder) had the majority of the functionality to get started. However, it did
not have the ability to create some core components like: Aggregate Targets,
Copy Headers Build Phases, Run Script Build Phases.  At least it did not until 
this [weekend](https://github.com/rayh/xcoder/pull/24).

I have created a [gist](https://gist.github.com/2201621) that will generate a 
universal framework with sources and header files. This should successfully 
boil the the multiple page [instructions](http://db-in.com/blog/2011/07/universal-framework-iphone-ios-2-0/) into roughly 100 lines of ruby code.

Perhaps the most valuable part is the ability to set the multitude of required
build settings, previously requiring deft configuration hunter skills, simply
with the following:

```ruby
target.create_configurations :release do |config|
  config.always_search_user_paths = false
  config.architectures = [ "$(ARCHS_STANDARD_32_BIT)", 'armv6' ]
  config.copy_phase_strip = true
  config.dead_code_stripping = false
  config.debug_information_format = "dwarf-with-dsym"
  config.c_language_standard = 'gnu99'
  config.enable_objc_exceptions = true
  config.generate_debugging_symbols = false
  config.precompile_prefix_headers = false
  config.gcc_version = 'com.apple.compilers.llvm.clang.1_0'
  config.warn_64_to_32_bit_conversion = true
  config.warn_about_missing_prototypes = true
  config.install_path = "$(LOCAL_LIBRARY_DIR)/Bundles"
  config.link_with_standard_libraries = false
  config.mach_o_type = 'mh_object'
  config.macosx_deployment_target = '10.7'
  config.product_name = '$(TARGET_NAME)'
  config.sdkroot = 'iphoneos'
  config.valid_architectures = '$(ARCHS_STANDARD_32_BIT)'
  config.wrapper_extension = 'framework'
  config.info_plist_location = ""
  config.prefix_header = ""
  config.save!
end
```