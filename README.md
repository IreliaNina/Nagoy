# Nagoy
Simple http client wrapper for WinInet

Include the header file into your project and link the library. 
The library is compiled static for x64. Below is a sample on how to use the library.

```cpp
#include <iostream>
#include "Nagoy.hpp"
#pragma comment(lib,"Http")
int main()
{
	std::unique_ptr<NagoyNS::Nagoy> ref = NagoyNS::createref();

	if (!NagoyNS::nagoyconnect(ref, "postman-echo.com", NagoyNS::NagoyConnectionPort::kInternetPortWithSSL, NagoyNS::NagoyServiceType::kServiceHTTP))
		return -1;
	NagoyNS::nagoypushheader(ref, "accept-encoding", "none");
	NagoyNS::nagoypushheader(ref, "Content-Type", "application/x-www-form-urlencoded");
	std::string query = "name=yousopunny";
	if (!NagoyNS::nagoyrequest(ref, "/post", NagoyNS::RequestTypePost(), query, true))
		return -1;

	NagoyNS::NagoyDataArc data = NagoyNS::readresponseheaders(ref);
	data->data[data->length-1] = 0;
	std::cout << data->data;
	NagoyNS::NagoyDataArc body = NagoyNS::readrequest(ref);
	body->data[body->length-1] = 0;
	std::cout << body->data;
	return 0;
}
```
