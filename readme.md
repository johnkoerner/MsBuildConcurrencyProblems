# Repoistory Purpose

This repository demonstrates an [issue](https://github.com/Microsoft/msbuild/issues/2366) with concurrency in builds with projects using the old and new project file formats.  In this case `B.CppCLILibrary` gets built twice.

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

# Demonstrating the Issue

Run `build.ps1`. This will run a concurrent build and will likely fail with a concurrency issue such as: 

```
c:\dev\msbuildconcurrencyproblems\b.cppclilibrary\assemblyinfo.cpp(1): fatal error C1041: cannot open program database 'C:\dev\MsBuildConcurrencyProblems\B.CppCLILibrary\x64\Debug\vc141.pdb'; if multiple CL.EXE write to the same .
       PDB file, please use /FS [C:\dev\MsBuildConcurrencyProblems\B.CppCLILibrary\B.CppCLILibrary.vcxproj]
```
or 
```
c:\dev\msbuildconcurrencyproblems\b.cppclilibrary\assemblyinfo.cpp(1): fatal error C1093: API call 'ImportFile' failed '0x80070020': ErrorMessage: The process cannot access the file because it is being used by another process. [C:
       \dev\MsBuildConcurrencyProblems\B.CppCLILibrary\B.CppCLILibrary.vcxproj]
```

