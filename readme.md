# Repoistory Purpose

This repository demonstrates a an [issue](https://github.com/Microsoft/msbuild/issues/2366) with concurrency in builds with projects using the old and new project file formats.  In this case `B.CppCLILibrary` gets built twice.

## Project Setup

This solution contains 3 projects:

 - A.NetFramework.Console
 - B.CppCLILibrary
 - C.DualTargetLibrary

In this case the references are as follows:

    A=>B
    C=>B

### A.NetFramework.Console

This project is a standard .Net 4.7.1 project using the old csproj file format. It references `B.CppCLILibrary` unconditionally

### B.CppCLILibrary

This is a C++ project that is referenced by `A.NetFramework.Console` and `C.DualTargetLibrary`

### C.DualTargetLibrary

This is a C# project that uses the new csproj file format. It targets both `net471` and `netstandard2.0` and has a conditional reference to  `B.CppCLILibrary` when built for `net471`.


