# check_chrony

Icinga2 / Nagios check for Chrony NTP & PTP offset to reference. Performance data is also included and no sudo privileges are required for execution.
No additional languages like perl or python required. Written on bash

## Example
```bash
# /usr/local/nagios/plugins/check_chrony -w 1 -c 3
OK: Time offset of +0.000046626 seconds to reference. |offset=0.000046626s;1.000000000;3.000000000
```

## Requirements
Bash

### Tested on
* Debian 11 - 12
* Ubuntu 22 - 24.04

### Parameters
* -w [Warning threshold] : (required) in seconds 
* -c [Critical threshold] : (required) in seconds 
* --debug : debug output

## Notes
Full documentation will follow soon.

## License
This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.
This program is distributed hoping that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses/
