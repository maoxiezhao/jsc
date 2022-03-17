# jsc
A more flexible file format based on json
  
Feature:
* Support inheritance
* Support variables
  
Example:
I used jsc to create a cfg for vulkan-test
```
import "../tools/cjing_build/config_base.jsc"
{
    jcs_def :
    {
        sln_name : "VulkanTest",
        cjing_build_dir : "../tools/cjing_build",
        assets_dir : "../assets",
        assets_export_dir : ".export",
    },

    ///////////////////////////////////////////////////////////////////
    // platform:win32
    win32 :
    {
        // custom visual studio dir
        custom_vs_dir_path : "C:\\Program Files (x86)\\Microsoft Visual Studio",

        // source assets directory
        jcs_def : 
        {
            platforms : "win32"
        },

        // premake
        premake : {
            args : [
                "%{vs_version}",   // To genenrate vs sln, the first param must is "vs_version"
                "--sln_name=${sln_name}",
                "--env_dir=../",
                "--work_dir=${assets_dir}",
                "--renderer=vulkan",
                "--platform_dir=win32",
                "--sdk_version=%{windows_sdk_version}",
            ]
        },

        // copy
        copy : {
            type : copy,
            explicit: true,
            files : [
                ["${assets_dir}/${assets_export_dir}", "bin/${platforms}/${assets_export_dir}"],
            ]
        },

        // build
        build : {
            type : build,
            explicit: true,
            buildtool: "msbuild",
            files : [
                "build/${platforms}/${sln_name}.sln"
            ]       
        },

        // launch
        launch : {
            type : shell,
            explicit: true,
            commands : [
                 "bin\\${platforms}"
            ],
            // copy
            copy : {
                files : [
                    ["${assets_dir}/${assets_export_dir}", "bin/${platforms}/${assets_export_dir}"],
                ]
            },
        }
    }
}
```
