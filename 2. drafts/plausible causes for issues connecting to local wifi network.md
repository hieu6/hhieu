**quick glitches**
- good old `turn it off and turn it back on again`
- a corrupted saved network entry.[^2] forget the network and rejoin to clear it: `System Settings > Wifi > Details > Forget This Network`.

**network issues**
- the router needs to be restarted, or has a DHCP problem[^3] ("Connected, no internet")
- the network is a captive portal[^1] (hotel, cafe) and the login page isn't loading. Try opening `http://captive.apple.com` in a browser[^4]

**macOS settings and software**
- outdated macOS with a known wifi bug (check Software Update)
- private wifi address (randomized MAC[^5]) conflicting with the router. set it to `Off` for that network to test
- bad DNS settings or a leftover custom DNS/proxy configuration[^6], which can look like "connected but nothing loads"
- a VPN, firewall, security or content filter app interfering with the connection

[^1]: A captive portal controls access to wifi networks by redirecting users to a login page before granting internet access.

[^2]: what causes a corrupted saved network entry?

[^3]: what is DHCP and what does it do? how do you check how to fix that?

[^4]: what does this site do?

[^5]: what is a randomized MAC? why does it randomize? what situations would you want to set this to `On`?

[^6]: there was something about DNS settings between Netlify and Porkbun to host a website. what is DNS for, why would you customize it vs. leaving it standard? what is a proxy and how is it related to DNS?
