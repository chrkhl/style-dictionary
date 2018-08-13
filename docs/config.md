# Configuration

Style dictionaries are configuration driven. Your config file defines what executes and what to output when the style dictionary builds.

By default, Style Dictionary looks for a config.json file in the root of your package. You can also specify a custom location when you use the [CLI](how_to_build#cli). If you are using a [custom build system](how_to_build#custom-build-system), you can specify a custom location for a configuration file or use an inline configuration JSON object.

## config.json
 Here is a quick example:
```json
{
  "source": ["properties/**/*.json"],
  "platforms": {
    "scss": {
      "transformGroup": "scss",
      "prefix": "sd",
      "buildPath": "build/scss/",
      "files": [{
        "destination": "_variables.scss",
        "format": "scss/variables"
      }],
      "actions": ["copy_assets"]
    },
    "android": {
      "transforms": ["attribute/cti", "name/cti/snake", "color/hex", "size/remToSp", "size/remToDp"],
      "buildPath": "build/android/src/main/res/values/",
      "files": [{
        "destination": "style_dictionary_colors.xml",
        "template": "android/colors"
      }]
    }
  }
}
```

| Attribute | Type | Description |
| :--- | :--- | :--- |
| source | Array[String] | An array of paths to JSON files that contain style properties. The Style Dictionary will do a deep merge of all of the JSON files so you can separate your properties into multiple files. |
| platforms | Object | An object containing platform config objects that describe how the Style Dictionary should build for that platform. You can add any arbitrary attributes on this object that will get passed to formats/templates and actions (more on these in a bit). This is useful for things like build paths, name prefixes, variable names, etc.  |
| platform.transforms | Array[String] (optional) | An array of [transforms](transforms.md) to be performed on the style properties object. These will transform the properties in a non-desctructive way so each platform can transform the properties. Transforms to apply sequentially to all properties. Can be a built-in one or you can create your own. |
| platform.transformGroup | String (optional) | A string that maps to an array of transforms. This makes it easier to reference transforms by grouping them together. You must either define this or `transforms`. |
| platform.buildPath | String (optional) | Base path to build the files, must end with a trailing slash. |
| platform.files | Array (optional) | Files to be generated for this platform. |
| platform.file.destination | String (optional) | Location to build the file, will be appended to the buildPath. |
| platform.file.format | String (optional) | [Format](formats.md) used to generate the file. Can be a built-in one or you can create your own. Must declare a format or a template. |
| platform.file.template | String (optional) | [Template](templates.md) used to generate the file. Can be a built-in one or you can create your own. |
| platform.actions | Array[String] (optional) | [Actions](actions.md) to be performed after the files are built for that platform. Actions can be any arbitrary code you want to run like copying files, generating assets, etc. You can use pre-defined actions or create custom actions. |

----