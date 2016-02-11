# checkprovider
checkprovider - Generate timing information for HTTP requests.

# Description
checkprovider returns a breakdown of timing information for requests to
a backend By default, the script runs 10
queries at a rate of one per second, and outputs timing information for each
request to stdout. 

The timing results are presented in tabulated form, with the following column
headings: (all times are in milliseconds)

## Timestamp

The timestamp of when the request started

## DNS

The time taken to perform the DNS lookup for the server.

## Socket

The time required to create a socket for this request.

## Connect

The time taken to connect to the TCP endpoint.

## TLS

The time taken to complete the TLS handshake.

## Write

The time spent writing the HTTP request to the socket.

## 1st Byte

This is the time spent waiting for the first byte of the response.

## Read

The total time spent reading the response from the HTTP server.

## Close

The time to close the socket after reading the full response.

## Total

The total time elapsed during the request.

## Bytes

The number of bytes read in the response (including response headers).

## Status

This column shows either `OK` or `NOT OK` to indicate whether the request
was successful (i.e. returned an HTTP 200 response). If any other response
was obtained, or the initial connection was unsuccessful then all fields from
`Connect` through to `Close` will be empty.

checkprovider will die with an error if the initial DNS lookup or socket creation fail.

## Term

This column shows the query term used for the request.

## Example

Here is an example of the output:

<pre>
$ checkprovider https://www.yahoo.com/
                                                      Request Times (ms)
       Timestamp             DNS   Socket  Connect      TLS    Write  1stByte     Read    Close    Total   Bytes Status
2016-01-30 06:54:53 UTC     1.02     0.10     1.54    19.38     0.10    67.87   227.48     0.32   318.14  432019 OK (200)
2016-01-30 06:54:54 UTC     1.15     0.13     1.31    18.40     0.10    70.84   403.08     0.24   495.38  439123 OK (200)
2016-01-30 06:54:55 UTC     1.07     0.10     1.47    18.15     0.09    66.33   234.07     0.22   321.65  437772 OK (200)
2016-01-30 06:54:56 UTC     0.98     0.09     1.39    19.93     0.09    65.90   258.00     0.34   346.88  433950 OK (200)
2016-01-30 06:54:57 UTC     0.99     0.10     1.32    17.78     0.09   104.22   268.77     0.26   393.73  433018 OK (200)
2016-01-30 06:54:58 UTC     1.05     0.10     1.42    18.88     0.06    78.30   279.99     0.22   380.18  433947 OK (200)
2016-01-30 06:54:59 UTC     1.01     0.09     1.42    17.44     0.10    63.76   241.61     0.30   325.90  441893 OK (200)
2016-01-30 06:55:00 UTC     1.27     0.12     1.38    19.11     0.07    74.45   279.24     0.51   376.29  439629 OK (200)
2016-01-30 06:55:01 UTC     8.97     0.00     1.69    12.00     0.00    85.00   213.00     6.00   326.96  439342 OK (200)
2016-01-30 06:55:02 UTC     1.00     0.00     2.00    23.01     0.00    68.99   264.00     1.00   360.00  440608 OK (200)
</pre>

# Dependencies

The current version is written in Perl and depends on the following non-core modules:
 * IO::Socket::SSL

The following core modules are also required:
 * File::Basename
 * Getopt::Long
 * Pod::Usage
 * POSIX
 * Socket
 * Time::HiRes

The folloring modules are optional:
 * URI::Encode (Note: only used for `-T` option)

# Installation

For now this code is not packaged (see also TODO). To use you install the dependencies and then fetch this repo. For example on redhat-based systems:

    $ sudo yum install perl-IO-Socket-SSL perl-URI-Encode
    $ git clone -o upstream git@github.com:yahoo/checkprovider.git
    $ export PATH=$PATH:`pwd`/checkprovider

# Options

The following command line options are supported by checkprovider:

<dl>
<dt>--no-blank</dt>
<dd>Suppress the blank line at the end of the list of timing results.</dd>

<dt>-d|--debug</dt>
<dd>Print some debug information to stdout.</dd>

<dt>-F feed|--feed feed</dt>
<dd>Set the "feed" part of the url, eg -f /forecastxml</dd>

<dt>-f frequency|--frequency frequency</dt>
<dd>Set the frequency of the requests in seconds. This is the inverse of the query rate, and takes precendence over any rate specified with `--rate`.</dd>

<dt>-h|--no-header</dt>
<dd>Suppress the display of header at the top of the list of timing results.</dd>

<dt>-H header|--header header</dt>
<dd>Specify an additional HTTP request header. Note: this option can be specified multiple times to provide multiple request headers.</dd>

<dt>-?|--help</dt>
<dd>Display full help information.</dd>

<dt>-p period|--period period</dt>
<dd>This specifies the period, in whole seconds, over which the requests are to be made. The default is 10 seconds.</dd>

<dt>-r rate|--rate rate</dt>
<dd>This specifies the integer rate of requests per second that should be made by checkprovider. The rate and period determine the number of requests sent to the HTTP server. The default rate is 1 request per second.</dd>

<dt>-Q|--query string</dt>
<dd>The query part of the URL, eg. `-q "q=london"`.</dd>

<dt>-q|--quiet</dt>
<dd>Suppress the blank line at the end of the results as well as the header. This has the same effect as using `--no-header` and `--no-blank`.</dd>

<dt>-T|--termsfile filename</dt>
<dd>A file containing a list of terms to be used in place of the `[TERM]` string in the URL. The format of the file is one term per line.</dd>

<dt>-v|--version</dt>
<dd>Display the version information then quit.</dd>

<dt>-w host|--host host</dt>
<dd>Specify the HTTP Host header.</dd>

<dt>-U useragent|--useragent useragent</dt>
<dd>Specify the User-Agent HTTP header.</dd>
</dl>

# TODO

This is a work in progress and we welcome pull requests. Here are a few things that still need work:

 * Packaging (rpm/deb/other?)
 * Protocol support (HTTP/1.0 and HTTP/2)
 * pretty graph generation

# License

Code licensed under the Apache License Version 2.0. See LICENSE file for terms.

# Contributors

 * Richard Misenheimer
 * Ian Brayshaw
 * Leong-Kui Lim
 * Peter Ellehauge
 * Scott Beardsley
