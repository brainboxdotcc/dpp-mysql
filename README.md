# A simple asynchronous MySQL wrapper for D++ bots

Simply take the source files and add them to your D++ bot project. This is a slightly modified version of what is used in my own bots.

This wrapper supports both synchronous (blocking) API and asynchronous (coroutine) API. All queries done through this wrapper use cached prepared statements, this will consume a very small amount of ram for a sometimes drastic increase in performance.

It is thread safe, however be aware that different threads may run queries that may intrude into other threads transactions. If you need ACID transaction safety, you should make sure that transactional queries are all run off the same thread using the blocking API. Asynchronous APIs by their nature will struggle with this!

No support is offered for this software at present. Your mileage may vary. I have only tested this wrapper on Linux. Detecting and linking the dependencies is currently your responsibility.

## Dependencies

* libmysqlclient-dev
* D++
* fmtlib
* A C++ compiler capable of building D++ bots with coroutine support, if you want to use the asynchronous interface

## Documentation

All functions in the `db` namespace have Doxygen comment blocks.

## Using the wrapper

This is an example of using the asynchronous interface:

```cpp
#include <dpp/dpp.h>
#include "database.h"
#include "config.h"

int main(int argc, char const *argv[]) {
	config::init("config.json");
	dpp::cluster bot(config::get("token"));

	bot.on_ready([&bot](const dpp::ready_t& event) -> dpp::task<void> {
		auto rs = co_await db::co_query("SELECT * FROM bigtable WHERE bar = ?", { "baz" });
		std::cout << "Number of rows returned: " << rs.size() << "\n";
		if (!rs.empty()) {
			std::cout << "First row 'bar' value: " << rs[0].at("bar") << "\n";
		}
		co_return;
	});

	db::init(bot);
	bot.start(dpp::st_wait);
}
```

Also create a `config.json` file. To use unix sockets to connect, set the port value to 0.

```json
{
    "token": "discord bot token",
    "database": {
            "host": "hostname",
            "username": "database username",
            "password": "database password",
            "database": "schema name",
            "port": 0,
            "socket": "/path/to/mysqld.sock"
    }
}
```
