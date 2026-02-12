# Kob-Web (W.I.P)
## A blazingly fast Rust Webserver built with Actix+mlua, that supports Lua as a backend scripting language with support to LuaRocks.


## How does it work?
So to use Kob-web you first need to have the following structure:
```
(config, views and static are yet to be implemented.)
server/
|-----config
|         |------server.toml
|
|-----logic
|         |------index.lua
|
|-----templates
|         |------index.html
|
|-----static
           |------icon.png
```
To start using Kob-Web, create an index.lua, that file will be responsible for the backend login at the endpoint /.
After that you can script however you want with kob-web! There is an example on how to use it down below:
```lua
hi="Hello"
name="john"

---you need to always return something at the end of the script so that the endpoint renders output. 
return hello .. "my name is" .. name .. "today it is: " .. os.time()

```

## TODO List
- [x] Pass minimal request info onto the Lua VM (Request method: "GET, POST, PUT...", clientip, url).
- [ ] Pass params and header-type of the request to the Lua VM, and allow Lua VM to modify the response headers.
- [ ] Add support to a template engine (maybe Jinja2).
- [ ] Add static folder's functionality.
- [ ] Add config/server.toml functionality. 

## How to compile
### Linux:
Install cargo

Clone the repo

Go inside the folders.

And run 
```sh
cargo run
```
### Windows:
Same stuff :P.
