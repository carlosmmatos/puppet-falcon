# Reference

<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

### Classes

#### Public Classes

* [`falcon`](#falcon): configures and installs CrowdStrike Falcon Sensor

#### Private Classes

* `falcon::config`: This class handles the configuration of the falcon server.
* `falcon::install`: This class handles falcon sensor package.
* `falcon::params`: This class contains the defaults for the falcon module.
* `falcon::service`: This class handles falcon sensor service.

### Resource types

* [`falconctl`](#falconctl): Configure the Falcon Sensor
* [`sensor_download`](#sensor_download): Download the Falcon Sensor

### Functions

* [`falcon::sensor_download_info`](#falconsensor_download_info): Get sensor info like install package SHA and version

## Classes

### <a name="falcon"></a>`falcon`

configures and installs CrowdStrike Falcon Sensor

#### Examples

##### Basic usage

```puppet
class { 'falcon':
  cid            => '12345',
  client_id      => '<client_id>',
  client_secret  => '<client_secret>',
  update_policy  => 'platform_default'
  install_method => 'api'
}
```

#### Parameters

The following parameters are available in the `falcon` class:

* [`package_manage`](#package_manage)
* [`config_manage`](#config_manage)
* [`service_manage`](#service_manage)
* [`cid`](#cid)
* [`install_method`](#install_method)
* [`client_id`](#client_id)
* [`client_secret`](#client_secret)
* [`version_manage`](#version_manage)
* [`falcon_cloud`](#falcon_cloud)
* [`update_policy`](#update_policy)
* [`sensor_tmp_dir`](#sensor_tmp_dir)
* [`version`](#version)
* [`version_decrement`](#version_decrement)
* [`cleanup_installer`](#cleanup_installer)
* [`provisioning_token`](#provisioning_token)
* [`package_name`](#package_name)
* [`package_options`](#package_options)
* [`service_enable`](#service_enable)
* [`service_name`](#service_name)
* [`service_ensure`](#service_ensure)

##### <a name="package_manage"></a>`package_manage`

Data type: `Optional[Boolean]`

Whether to install and manage the `falcon sensor`. Defaults to `true`.

Default value: `$falcon::params::package_manage`

##### <a name="config_manage"></a>`config_manage`

Data type: `Optional[Boolean]`

Whether to manage the `falcon sensor` configuration. Defaults to `true`.

Default value: `$falcon::params::config_manage`

##### <a name="service_manage"></a>`service_manage`

Data type: `Optional[Boolean]`

Whether to manage the service. Defaults to `true`.

> **_NOTE:_**  The falcon service requires the agent to be registered with the Customer CID in order to start.

Default value: `$falcon::params::service_manage`

##### <a name="cid"></a>`cid`

Data type: `Optional[String]`

The Customer CID to register the agent with. If not provided, the agent will not be registered. The falcon service can not be started
if cid is not configured. Defaults to `undef`.

Ignored if `config_manage` is set to `false`.

Default value: `$falcon::params::cid`

##### <a name="install_method"></a>`install_method`

Data type: `Optional[Enum['api', 'local']]`

The method used to install the `falcon sensor`. Defaults to `api`.

Valid values:
  - `api`
  - `local`

When `api` is selected, the falcon api will be used to download the correct version of the falcon sensor.

When `local` is selected, a package resource is created with the values passed in the `package_options` parameter.

Default value: `$falcon::params::install_method`

##### <a name="client_id"></a>`client_id`

Data type: `Optional[Sensitive]`

The client id used to authenticate with the Falcon API. Defaults to `undef`.

Required if `install_method` is set to `api` and ignored if `install_method` is set to `local`.

Default value: `$falcon::params::client_id`

##### <a name="client_secret"></a>`client_secret`

Data type: `Optional[Sensitive]`

The client secret used to authenticate with the Falcon API. Defaults to `undef`.

Required if `install_method` is set to `api` and ignored if `install_method` is set to `local`.

Default value: `$falcon::params::client_secret`

##### <a name="version_manage"></a>`version_manage`

Data type: `Optional[Boolean]`

Rather or not puppet should enforce a specific version and do upgrades/downgrades. Defaults to `false`.

Ignored if `install_method` is set to `local`.

> **_NOTE:_**  If you use update policies to manage the version, you should set this to `false` to prevent puppet
  and the falcon platform from conflicting.

Default value: `$falcon::params::version_manage`

##### <a name="falcon_cloud"></a>`falcon_cloud`

Data type: `String`

The name of the cloud to use for the Falcon API. Defaults to `api.crowdstrike.com`

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::falcon_cloud`

##### <a name="update_policy"></a>`update_policy`

Data type: `Optional[String]`

The update policy to use to determine the package version to download and install. Defaults to `undef`.

`update_policy` takes precedence over `version_decrement`.

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::update_policy`

##### <a name="sensor_tmp_dir"></a>`sensor_tmp_dir`

Data type: `Optional[String]`

The directory to use to stage the sensor package. Defaults to `/tmp` (or `%TEMP%` on Windows).

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::sensor_tmp_dir`

##### <a name="version"></a>`version`

Data type: `Optional[String]`

The version of the sensor to install. When provided `update_policy` and `version_decrement` will be ignored. Defaults to `undef`.

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::version`

##### <a name="version_decrement"></a>`version_decrement`

Data type: `Optional[Numeric]`

The number of versions to decrement from the latest version. When `version`, `update_policy` are not provided
this will be used to determine the version to download and install. Defaults to `0`.

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::version_decrement`

##### <a name="cleanup_installer"></a>`cleanup_installer`

Data type: `Optional[Boolean]`

Rather or not to remove the sensor install package after use. Defaults to `true`.

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::cleanup_installer`

##### <a name="provisioning_token"></a>`provisioning_token`

Data type: `Optional[String]`

The provisioning token to use to register the sensor with the Falcon API. Defaults to `undef`.

Default value: `$falcon::params::provisioning_token`

##### <a name="package_name"></a>`package_name`

Data type: `Optional[String]`

The name of the package to install. Defaults to the valid service name for the OS.

`package_options` will override if you pass in a package name.

Ignored if `install_method` is set to `local`.

Default value: `$falcon::params::package_name`

##### <a name="package_options"></a>`package_options`

Data type: `Hash[String, Any]`

Allows you to override any package attribute. Defaults to `{}`.

Default value: `$falcon::params::package_options`

##### <a name="service_enable"></a>`service_enable`

Data type: `Optional[Boolean]`

Whether to enable the service. Defaults to `true`.

Ignored if `service_manage` is set to `false`.

Default value: `$falcon::params::service_enable`

##### <a name="service_name"></a>`service_name`

Data type: `Optional[String]`

The name of the service to manage. Defaults to the valid service name for the OS.

Ignored if `service_manage` is set to `false`.

Default value: `$falcon::params::service_name`

##### <a name="service_ensure"></a>`service_ensure`

Data type: `Optional[String]`

The desired service state. Defaults to `running`.

Ignored if `service_manage` is set to `false`.

Default value: `$falcon::params::service_ensure`

## Resource types

### <a name="falconctl"></a>`falconctl`

Configure the Falcon Sensor

#### Properties

The following properties are available in the `falconctl` type.

##### `cid`

The cid to set for the Falcon Sensor

#### Parameters

The following parameters are available in the `falconctl` type.

* [`name`](#name)
* [`provider`](#provider)
* [`provisioning_token`](#provisioning_token)

##### <a name="name"></a>`name`

namevar

The name of the resource

##### <a name="provider"></a>`provider`

The specific backend to use for this `falconctl` resource. You will seldom need to specify this --- Puppet will usually
discover the appropriate provider for your platform.

##### <a name="provisioning_token"></a>`provisioning_token`

The provisioning token used to register the sensor

Default value: ``undef``

### <a name="sensor_download"></a>`sensor_download`

Download the Falcon Sensor

#### Properties

The following properties are available in the `sensor_download` type.

##### `ensure`

Valid values: `present`, `absent`

The basic property that the resource should be in.

Default value: `present`

#### Parameters

The following parameters are available in the `sensor_download` type.

* [`bearer_token`](#bearer_token)
* [`falcon_cloud`](#falcon_cloud)
* [`file_path`](#file_path)
* [`provider`](#provider)
* [`sha256`](#sha256)
* [`version`](#version)
* [`version_manage`](#version_manage)

##### <a name="bearer_token"></a>`bearer_token`

The bearer token used to authenticate with the Falcon API

##### <a name="falcon_cloud"></a>`falcon_cloud`

The falcon cloud URI to use

##### <a name="file_path"></a>`file_path`

The full path to the file.

##### <a name="provider"></a>`provider`

The specific backend to use for this `sensor_download` resource. You will seldom need to specify this --- Puppet will
usually discover the appropriate provider for your platform.

##### <a name="sha256"></a>`sha256`

namevar

The sha256 of the package to download

##### <a name="version"></a>`version`

The falcon sensor version that should be installed.

##### <a name="version_manage"></a>`version_manage`

If true download the required sensor package if current sensor version does not match desired version. False only
download sensor package when no sensor is installed

## Functions

### <a name="falconsensor_download_info"></a>`falcon::sensor_download_info`

Type: Ruby 4.x API

Get sensor info like install package SHA and version

#### Examples

##### Calling the function

```puppet
falcon::sensor_download_info('client_id', 'client_secret', { 'falcon_cloud' => 'api.crowdstrike.com'})
```

#### `falcon::sensor_download_info(Sensitive $client_id, Sensitive $client_secret, Hash $options)`

Get sensor info like install package SHA and version

Returns: `Hash` download information about the sensor

- `sha256` the SHA256 checksum of the sensor package
- `version` the version of the sensor package
- `os_name` the name of the operating system the sensor is for
- `file_path` the fully qualified file path to download the sensor package to
- `bearer_token` the bearer token used to authenticate with the Falcon API
- `platform_name` the name of the platform the sensor is for

##### Examples

###### Calling the function

```puppet
falcon::sensor_download_info('client_id', 'client_secret', { 'falcon_cloud' => 'api.crowdstrike.com'})
```

##### `client_id`

Data type: `Sensitive`

the client id used to authenticate with the Falcon API

##### `client_secret`

Data type: `Sensitive`

the client secret used to authenticate with the Falcon API

##### `options`

Data type: `Hash`

used to determine how download information is retrieved

- `version` the version of the sensor to use
- `falcon_cloud` the name of the cloud to use
- `update_policy` the update policy to use
- `sensor_tmp_dir` the temporary directory to use
