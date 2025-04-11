# fluent-plugin-parser_cefalt

[![Gem Version](https://badge.fury.io/rb/fluent-plugin-parser_cefalt.svg)](https://badge.fury.io/rb/fluent-plugin-parser_cefalt)
[![downloads](https://img.shields.io/gem/dt/fluent-plugin-parser_cefalt.svg)](https://rubygems.org/gems/fluent-plugin-parser_cefalt)
[![MIT License](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](LICENSE)

Fluentd Parser plugin to parse CEF - common event format - Alternative

## Requirements

| fluent-plugin-parser_cefalt  | fluentd | ruby |
|---------------------------|---------|------|
| >= 1.0.0 | >= v0.14.0 | >= 2.1 |
|  < 1.0.0 | >= v0.12.0 | >= 1.9 |

## Installation

Add this line to your application's Gemfile:

```bash

# for fluentd v0.14 or higher

fluent-gem install fluent-plugin-parser_cefalt

```

## Usage

```
<source>
  @type syslog
  port 514
  bind 0.0.0.0
  <transport tcp>
  </transport>
  <parse>
    @type cefalt
    log_format syslog
    parse_strict_mode true
    output_raw_field false
  </parse>
  tag ceflog
</source>
<match ceflog.**>
  @type stdout
</match>
```

## parameters

* `log_format` (default: syslog)

  input log format, currently only 'syslog' is valid

* `log_utc_offset` (default: nil)

  set log utc_offset if each record does not have timezone information and the timezone is not local timezone

  if log_utc_offset set to nil or invalid value, then use system timezone

  if a log have timezone information, log_utc_offset is ignored

* `syslog_timestamp` (default: '\w{3}\s+\d{1,2}\s\d{2}:\d{2}:\d{2}')

* `syslog_timestamp_rfc5424` (default: '\d{4}[-]\d{2}[-]\d{2}[T]\d{2}[:]\d{2}[:]\d{2}(?:\.\d{1,6})?(?:[+-]\d{2}[:]\d{2}|Z)')

* `cef_version` (default: 0)

  CEF version, this should be 0

* `parse_strict_mode` (default: true)

  if the CEF extensions are the following, the value of the key cs2 should 'foo hoge=fuga'

  - cs1=test cs2=foo hoge=fuga cs3=bar

  if parse_strict_mode is false, this is raugh parse, so the value of the key cs2 become 'foo' and non CEF key 'hoge' shown, and the value is 'fuga'

* `cef_keyfilename` (default: 'config/cef_version_0_keys.yaml')

  used when parse_strict_mode is true, this is the array of the valid CEF keys

* `output_raw_field` (default: false)

  append {"raw":\<message itself\>} key-value even if success parsing

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
