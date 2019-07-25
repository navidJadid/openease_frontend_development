# TESTING-CATALOGUE
This file contains a checklist of directories and modules in this repository which still need to be tested. We will rank them on a scale from ![one][one_16] to ![five][five_16] for the degree of difficulty for testing. Please add the ![green_tick][green_tick_16] icon behind every module that has been tested, and the ![black_tick][black_tick_16] icon behind every tested method.  
If a module or method does not need testing, we mark it with a ![stop][stop_16] icon.

If a module is refactored, please consider indicating the changes in this file.

- ![folder][folder_16] Webrob
    - ![folder][folder_16] config
        - ![python][python_16] settings.py ![stop][stop_16]
    - ![folder][folder_16] docker
        - ![python][python_16] docker_application.py
        - ![python][python_16] docker_interface.py
    - ![folder][folder_16] models
        - ![python][python_16] db.py
        - ![python][python_16] experiments.py
        - ![python][python_16] teaching.py
        - ![python][python_16] tutorials.py
        - ![python][python_16] users.py
    - ![folder][folder_16] pages
        - ![python][python_16] api.py
        - ![python][python_16] db.py
        - ![python][python_16] editor.py
        - ![python][python_16] experiments.py
        - ![python][python_16] knowrob.py
        - ![python][python_16] login.py
        - ![python][python_16] meshes.py
        - ![python][python_16] mongo.py
        - ![python][python_16] oauth.py
        - ![python][python_16] tutorials.py
    - ![folder][folder_16] startup
        - ![python][python_16] init_app.py
        - ![python][python_16] init_db.py
        - ![python][python_16] init_webapp.py
    - ![folder][folder_16] utility
        - ![python][python_16] db_connection_checker.py
        - ![python][python_16] directory_handler.py
        - ![python][python_16] file_handler.py
        - ![python][python_16] path_handler.py
        - ![python][python_16] random_string_builder.py
        - ![python][python_16] system_environment_variable_getter.py
        - ![python][python_16] template_file_copyer.py
        - ![python][python_16] utility.py
    - ![python][python_16] app_and_db.py
- ![python][python_16] runserver.py
    - `_config_is_debug()`
    - `_run_debug_server()`
    - `_run_server()`
    - `__main__()`

**Icon References:**
- [![one][one_16] ![two][two_16] ![three][three_16] ![four][four_16] ![five][five_16]](https://www.flaticon.com/packs/characters-and-numbers) Icons made by [Twitter](https://www.flaticon.com/authors/twitter) from [www.flaticon.com]() are licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)
- [![stop][stop_16]](https://www.flaticon.com/free-icon/stop_151955#term=stop&page=1&position=18) Icons made by [Chanut](https://www.flaticon.com/authors/chanut) from [www.flaticon.com]() are licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)
- [![python][python_16]](https://www.flaticon.com/free-icon/python_919852#term=python&page=1&position=4) Icons made by [Freepik](https://www.flaticon.com/authors/freepik) from [www.flaticon.com]() are licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)
- [![folder][folder_16]](https://www.flaticon.com/free-icon/folder_148947#term=folder&page=1&position=36) Icons made by [SmashIcons](https://www.flaticon.com/authors/smashicons) from [www.flaticon.com]() are licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)
- [![green_tick][green_tick_16]](https://www.flaticon.com/free-icon/checked_291201#term=tick&page=1&position=2) Icon made by [Maxim Basinski](https://www.flaticon.com/authors/maxim-basinski) from [www.flaticon.com]() is licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)
- [![black_tick][black_tick_16]](https://www.flaticon.com/free-icon/check-symbol_60731#term=tick&page=1&position=19) Icon made by [Google](https://www.flaticon.com/authors/google)  from [www.flaticon.com]() is licensed under [CC 3.0 BY.](https://creativecommons.org/licenses/by/3.0/)

(Click the icons to be directed to the source page)

[one_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/Numbers/one_16.png
[two_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/Numbers/two_16.png
[three_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/Numbers/three_16.png
[four_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/Numbers/four_16.png
[five_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/Numbers/five16.png
[stop_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/stop_16.png
[python_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/python_16.png
[python_24]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/python_24.png
[folder_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/folder_16.png
[folder_24]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/folder_24.png
[green_tick_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/green_check_16.png
[black_tick_16]: https://raw.githubusercontent.com/navidJadid/openease_webserver_development/master/Testing%20Catalogue%20Icons/black_check_16.png