You can run this project directly by running:

```shell
cmake -B build -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
cd build
make -j
```

Then the DynamoRio will be downloaded automatically and the CMAKE_EXPORT_COMPILE_COMMANDS=ON will generate the compile_commands.json file which assist clangd find the dependencies.
