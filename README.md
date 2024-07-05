# dpp-mysql
A simple asynchronous MySQL wrapper for D++ bots

Simply take the source files and add them to your D++ bot project.

This wrapper supports both synchronous (blocking) API and asynchronous (coroutine) API. All queries done through this wrapper use cached prepared statements, this will consume a very small amount of ram for a sometimes drastic increase in performance.

It is thread safe, however be aware that different threads may run queries that may intrude into other threads transactions. If you need ACID transaction safety, you should make sure that transactional queries are all run off the same thread using the blocking API. Asynchronous APIs by their nature will struggle with this!

No support is offered for this software at present. Your mileage may vary. I have only tested this wrapper on Linux. Detecting and linking the dependencies is currently your responsibility.

## Dependencies

libmysqlclient-dev
D++
A C++ compiler capable of building D++ bots with coroutine support, if you want to use the asynchronous interface

## Documentation

All functions in the `db` namespace have Doxygen comment blocks.
