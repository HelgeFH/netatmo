# netatmo.py

Python 3 API to retrieve data from the Netatmo connected [weather station](https://www.netatmo.com/product/weather/).

The library implements the [authentication](https://dev.netatmo.com/dev/resources/technical/guides/authentication/clientcredentials), the [token refresh](https://dev.netatmo.com/dev/resources/technical/guides/authentication/refreshingatoken) and the both weather station methods [Getstationdata](https://dev.netatmo.com/dev/resources/technical/reference/weatherstation/getstationsdata) and [Getmeasure](https://dev.netatmo.com/dev/resources/technical/reference/common/getmeasure).

Although Netatmo provides [samples](https://dev.netatmo.com/dev/resources/technical/samplessdks/codesamples#) written in Python, this library provides - I hope! - more high level methods to access the data.

## Requirements

* Python 3 (sorry if you live in [legacy](https://wiki.python.org/moin/Python2orPython3))
* The [requests](http://docs.python-requests.org/) module (should be included in any decent Python distribution)
* A valid Netatmo account with at least one weather station
* A client\_id / client\_secret pair from Netatmo developper program (see [Create your app](https://dev.netatmo.com/dev/createanapp))

## Command-line usage

### Help
    
    ./netatmo.py -h
    ./netatmo.py {command} -h
    
where `{command}` can be one of these keywords: `config`, `fetch`, `list`, `test`, `dump`.
    
### Credentials

The library reads the username/password and client id/secret from a .rc file. By default, it is ~/.netatmorc. It could be edited by hand, or written by the library with the `config` command.

    ./netatmo.py config -u user@mail -p password -i client_id -s client_secret -d 70:ee:50:xx:xx:xx
    
Without any option, `config` only prints the current configuration. 

    $ ./netatmo.py config
    Read config
    username: user@mail
    password: password
    client_id: 1234567890abcdef12345678
    client_secret: ABCdefg123456hijklmn7890pqrs
    default_station: 70:ee:50:xx:xx:xx
    $

### Display the authorized stations

    ./netatmo.py list
    
### Fetch data into CSV files

    ./netatmo.py fetch
    
This command will write two CSV files, `netatmo_station.csv` and `netatmo_module.csv`. The most recent measures are appended to these files depending on the last timestamps.
 
### Other commands and options
 
`test` tests the connection. On success, exit code is zero. On failure, non zero, like any shell command.

`dump` displays more data from the weather station.

Each option `-v` increases the verbosity. The option `-c` can be use to use an alternate configuration file.

Both `-v` and `-c` have to be placed before the command.

## Usage as a Python module

    #! /usr/bin/env python3
    
    import netatmo
    
    # fetch data using ~/.netatmorc credentials    
    netatmo.fetch()
    
    # credentials as parameters
    ws = netatmo.WeatherStation( {
           'client_id': '1234567890abcdef12345678',
           'client_secret': 'ABCdefg123456hijklmn7890pqrs',
           'username': 'user@mail',
           'password': 'password',
           'default_station': '70:ee:50:XX:XX:XX' } )
    ws.getData()
    print(ws.devices)
    
## License and warranty
 
None and none.

It is NOT an official software from Netatmo and it is not endorsed or supported by this company.

This library has been written as a personal work. Feel free to improve or adapt it to your own needs.

## Notes

### Other Netatmo devices

This library has been tested only with the weather station and its interior module. I don't know if it works well with the windgauge or the pluviometer. Other devices are unsupported, but their methods could be easily added. See [Netatmo Connect APIs](https://dev.netatmo.com/dev/resources/technical/reference).

### Installation on a Synology NAS

Synology provides a Python 3 package that lacks the [requests](http://python-requests.org/) module. Here is an simple download method, without git, pip or setup.py:

    curl -sL https://api.github.com/repos/kennethreitz/requests/tarball/v2.12.1 | tar -xzf - --strip-components=1 --wildcards '*/requests'

