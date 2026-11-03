# Kob-Web (v1.4-beta1)

## A blazingly fast Rust Webserver built with Actix+mlua, that supports Lua as a backend scripting language with support to LuaRocks.
> [!WARNING]
> Kob-Web is W.I.P at the time, several things may be subject to change!


## How does it work?
So to use Kob-web you first need to have the following structure:

```
server/
├── config/
│   └── server.toml
├── logic/
│   └── index.lua
├── templates/  (yet to be implemented, but accessible thru lua scripts at the momment.)
│   └── index.html
└── static/
    └── icon.png
```

To start using Kob-Web, create an index.lua, that file will be responsible for the backend login at the endpoint /.
After that you can script however you want with kob-web! There is an example on how to use it down below:
```lua
hi="Hello"
name="john"

---you need to always return something at the end of the script so that the endpoint renders output. 
return hello .. "my name is" .. name .. "today it is: " .. os.time()
```

## What's new?
<details>
<summary>Maximum Payload Size.</summary>
## Payload Size limit
\n
Since you now can parse request body's on kob-web, you may want to receive files sometimes,
and to set a maximum payload size, you can do so on the config/server.toml with the following key:
    
```toml
[server]           
max_payload_size=25
```
</details>

<details>
<summary>Request Info Parsing (Improved)</summary>

## Request Table **(Improved)**
Kob-Web already had support for minimal request info, which was improved,
the request table goes the following way:
 ```lua
request= {
  method = 404, -- Kob-web 404's by default
  path = "/authentication/login",
  socketip = "0.0.0.0", --client ip renamed ot socket ip, since it's the socket's ip that's returned and not the client's.
  headers = { -- Request Headers were implemented which allows you to see more about the request 
    ["content-type"] = "text/html", --Kob-web set's content-type as text/html by default
    foo = "bar"
  },
  body = "hi" -- Added Request body to parse input

}
```

</details>

<details>
<summary>Response Control</summary>

## Response Control
Kob-Web has a response table which is composed the following way:
 ```lua
response= {
  statuscode = 404, -- Kob-web 404's by default
  headers = {
    ["content-type"] = "text/html", --Kob-web set's content-type as text/html by default
    foo = "bar"
  }

}
```

</details>

<details>
<summary>Static Folder</summary>

## Static Folder

Kob-Web has a static folder to host and serve media and static content!

You can access it thru the /static route on your server

</details>


## Existing features / Mini Documentation

<details>

<summary>Path Parameters</summary>

## Path Params:

Kob-Web's way of finding the lua file for the route the client requests has changed to, for example:

If there is a request /users kobweb will try to find either
users.lua or users./index.lua
And there is now support for Path Params:

If there is a request `/users/45`  Kob-Web will try to find either `users/45.lua`or `users/45/index.lua`, if there is neither, kob-web will call `users.lua` or `users./index.lua` with path_params `"45"`, take note that kob-web takes priority on files over params, if there is a `45.lua` , it will have priority over `users.lua` or `users/index.lua` with `45` as a path parameter.

If you wish to have a route that accepts params, you need to specify it in server.toml, under the [routing] section with the key "allow_path_params" with the value of the routes you wish, for ex:
```toml
[routing]
allow_path_params = ["/test", "/admin/*]
# The input aboves allow the endpoint "/test" to take parameters. There also is support for wildcards as seen in "/admin/*", it will give all endpoints under /admin/ to support params, for example: "/admin/warn/1", "/admin/ban/2"
```
</details>

<details>
<summary>Query Parameters</summary>

## Query Params

Kob-Web supports standard query parameters just like any other web framework.

*Example:*
`/users?id=42&name=john&filter=active`


Query parameters are automatically parsed and passed to your Lua script as a table called query_params:
```lua
-- Access individual params
local user_id = query_params.id        -- "42"
local user_name = query_params.name    -- "john"
local filter = query_params.filter     -- "active"

-- Or iterate through all params
for key, value in pairs(query_params) do
    print(key .. " = " .. value)
end

return "User ID: " .. (query_params.id or "none")
```

Query parameters are available in all routes automatically, no configuration needed!

</details>


## TODO List
- [x] Pass minimal request info onto the Lua VM (Request method: "GET, POST, PUT...", clientip, url).
- [x] Add config/server.toml functionality. 
- [x] Pass path parameters and query parameters to the Lua VM
- [x] Pass Header-type of the request to the Lua VM, and allow Lua VM to modify the response headers.
- [x] Add static folder's functionality.

- [ ] Add support to a template engine (maybe Jinja2).

## How to build from source
### Linux:
(Arch linux)
Install lua:
```bash
pacman -S lua
```

Install cargo
```bash
pacman -S rustup
rustup install stable
rustup default stable
```

Clone the repo
```bash
git clone https://github.com/zauceee/kob-web.git
```

Go inside the kob-web folder.
```bash
cd kob-web
```

And run 
```bash
cargo build --release
```

The binary will be inside
```bash
cd ./target/release/
./kob-web
```

### Windows:
Same stuff :P.
