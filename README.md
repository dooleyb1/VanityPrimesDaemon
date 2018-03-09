# ServerSocketKnocking
C++ program that uses a _daemon_ to fork a process which creates a HTTP server which generates *"vanity primes"* upon request. This project makes use of [Civetweb](https://github.com/civetweb/civetweb) which is a lightweight HTTP library.

## Definitions & Important Notes

* A daemon is a program that forks a child process to do it's work and then the parent exits leaving the child process running as a server.

* A "vanity prime" is a (long) prime number the high order bits of which are constructed to match a supplied vanity string. 

* Vanity strings are limited to ASCII hex values, e.g. "0xdeadbeef" but may be of any length up to (nearly) 1024 bits long. 

## Setup

To begin with, clone the repo.
```
git clone https://github.com/dooleyb1/VanityPrimesDaemon
```

Explode the mbedtls tarball.
```
tar xzvf mbedtls-2.6.0-apache.tgz
```

Build the mbedtls code.
```
cd mbedtls-2.6.0
make
```

Go back up a level and compile the vanity_server program.
```
cd ..
make
```

Run the vanity prime generation server (locally on port 8081).
```
./vanity_server

Browse files at http://localhost:8081/
Run example at http://localhost:8081/example
Exit at http://localhost:8081/exit
Generate vanity prime at http://localhost:8081/.well-known/vanityprime?vs=0x...
Exit vanity prime at http://localhost:8081/vpexit
```

## Example Usage

**Open up a new terminal** and send a vanity prime generation request to the server and await response.

```
curl http://localhost:8081/.well-known/vanityprime?vs=0xdeadb33f

Vanity string = deadb33f
Vanity string length = 8 

Attempting to find vanity prime number...

Vanity prime found = deadb33fAK@F81B28F06AC0161D27EF6E60949BEF4AF2B47B7C8A928FF4B410FC5A6202D897AF70BD4660D1C37897BD9DF849DCB474EAD21923C9AC72E676580F060D87D12315C4F209048E3F02F32D5C4E6DB3D17E06F8187CC96D50E7D210B91E00BAF38CC7E8111D388A474A73B089E7B7AF3BF0B0582ADCBA99D2E8AA255

Attempts = 729
```

When finished using the server make sure to exit and close the server by sending the HTTP request as follows:

```
curl http://localhost:8081/vpexit
Vanity prime exit command received!
```

Sometimes the server is unable to generate a vanity prime number after a given number of attempts (1000). This will result in the following error code.

```
400 Bad Request
Attempts to produce vanity string exceeded max attempts (1000)Attempts = 1000
```
