# obj_strings

Helps Cocoa applications localization by detecting:

- warnings for untranslated strings in *.m
- warnings for unused keys in Localization.strings
- errors for keys defined twice or more in the same `.strings` file

### Usage

```bash
$ python objc_strings.py -p /path/to/project
./MyProject/en.lproj/Localizable.strings:13: warning: unused key in en.lproj: "Misc"
./MyProject/ViewController.m:16: warning: missing key in fr.lproj: "World"
```    

#### Project path finding order (one of them)

1. Set on option `./obj_strings.py -p project_path`
2. Environment variable `export PROJECT_PATH=project_path`
3. Script execution path

### Xcode integration

1. make `objc_strings.py` executable

```bash
$ chmod +x objc_strings.py
```

2. copy `objc_strings.py` to the root of your project
3. add a "Run Script" build phase to your target
4. move this build phase in second position
5. set the script path to `"${SOURCE_ROOT}/objc_strings.py"`

![settings](https://github.com/nst/objc_strings/raw/master/images/settings.png "settings")
![warnings](https://github.com/nst/objc_strings/raw/master/images/warnings.png "warnings")

### Common Issues

Some may experience *UnicodeDecodeError* when running the script.
The problem is that the script runs through all directories to look for .strings files, which may include already compile .strings files which can not be parsed. Often you have some in Build/ or if you integrate CocoaPods ( Pods/ )

To prevent this you can add dirs which you want to have excluded like this

```bash
"${SOURCE_ROOT}/objc_strings.py" --exclude-dirs=['Build','Pods']
```

or if you are on terminal

```bash
$ objc_strings.py --project-path /path/to/obj_c/project --exclude-dirs=['Build','Pods']
```

### ToDo

* Scan Interface Builder (.xib) Files for localized Strings
