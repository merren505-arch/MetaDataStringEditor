# Partial String Modification Tool for global-metadata.dat

For Android games exported from Unity's il2cpp script backend, strings appearing in the code are compiled into the `assets\bin\Data\Managed\Metadata\global-metadata.dat` file. As part of localization work, I created a simple tool to modify strings within this file.

## References

- [il2cppdumper](https://github.com/Perfare/Il2CppDumper)

Understanding of this file format was learned from the source code of this tool. Il2CppDumper itself is used to extract class definitions from the compiled `libil2cpp.so` file and `global-metadata.dat`. It supports various export formats including IDA-compatible renaming scripts and DLLs for UABE and AssetStudio. It's a very useful tool.

## How It Works

In `global-metadata.dat`, strings from code are stored as follows: the header contains a list with the offset, length, and other metadata for each string, followed by a data section that stores all the strings in a compact format. Since the header list maintains this information, null-termination is not required.

Because the number of strings remains unchanged before and after modification, the header list is modified by directly overwriting it in place. The data section length may change: if the modified data section is less than or equal to the original length, it overwrites the original data in place. If it exceeds the original length, the new data is written to the end of the file.
