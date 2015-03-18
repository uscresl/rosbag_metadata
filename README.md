# rosbag_metadata
Tool for collecting and writing metadata to ROS bagfiles or to accompanying
yaml files.

## System metadata

The tool collects various system metadata by default and saves along with the
user entered data. The purpose of this is to be able to figure out what
versions of software, os, etc. was running at the time of the bag recording.

Each category of system metadata can be disabled through command line options.

Currently, the following data is saved:

* ROS distribution info and package versions
* Information on git repositories within ```ROS_PACKAGE_PATH```
* Environmental variables
* Network interfaces and IP address information
* Connected USB devices

## Examples

### Reading

Display metadata from a bag file:

```rosbag_metadata mybag.bag```

Look for metadata in a directory:

```rosbag_metadata /path/to/dir```

First it searches for /path/to/dir/metadata.yaml, followed by
 inspecting .bag files for /metadata topic. Displays the first hit unless
```--find-all``` is specified.

Read matadata from a yaml file:

```rosbag_metadata file.yaml```

### Writing

Write data to a bag file:

```rosbag_metadata -w mybag.bag```

Write data to a yaml file:

```rosbag_metadata -w file.yaml -t templatefile.yaml```

Write data to directory:

```rosbag_metadata -w /path/to/dir --write-rosbag-info```

```--write-rosbag-info``` will cause ```.bag``` files in the directory to be
inspected with ```rosbag info``` and the resulting information will be added to
the metadata. This option does not work when the target is a bag file.

### Templates

Template files are simpy yaml files with key pairs that will be used as default
keys and values if defined. If a template is used the default behavior of
rosbag_metadata is to not ask for values of keys defined with defaults in the
template. This behavior can be overwritten using the ```--ask-template-defaults```
or per template item basis (see example template).

```
description: {value: My experiment, ask: 1}
operator: Mr. User
location: Building X
```

## Configuration

The template, default keys, system metadata settings and more can be configured
in a config file. By default, rosbag_metadata will look for a config file
```~/.ros/rosbag_metadata.conf```. Users can also specify a config file with the
```-c/--config``` option.

### Example config

The following config includes all the possible configuration options.
Default fields can be configured both via template and in the config file.

```
[config]
clean = no
write_rosbag_info = yes
system_info = yes
system_info_all = no
system_info_usb = yes
system_info_git = yes
system_info_ros = yes
system_info_env = yes
system_info_full_env = no
system_info_ip = yes
find_all = yes
debug = no
ask_template_defaults = no
template = ~/.ros/my_template.yaml
extra_fields = no

[default_fields]
my_default_field
another_field = with_value
```

## More Documentation:

http://rosbag-metadata.readthedocs.org/
