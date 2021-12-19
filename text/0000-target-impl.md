- Feature Name: `target-impl`
- Start Date: 2021-11-26
- RFC PR: [hive/rfc#0000](https://github.com/mscofield0/hive/pull/0000)
- Hive Issue: [hive#0000](https://github.com/mscofield0/hive/issues/0000)

# Summary
[summary]: #summary

Defines the target system, its structure and how it operates. 

# Motivation
[motivation]: #motivation

Refer to [RFC#0001](0001-target-semantics.md) for the motivation.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

I propose the following structure for Hive:

- **\<target-name>.hive** - Target file
- **\<name>.incl** - Arbitrary Hive include file (used to clean up target files by spreading out the functionality elsewhere)

## Targets

Hive should scan for all the targets in the current directory explicitly when the user asks him to. Every action besides scanning should require Hive to have metadata in the specified directory. This keeps things simple and automated without much hassle for the user. So, each time you add a target you would do a rescan to update Hive's target information.

When Hive is invoked to build or setup a target, Hive first checks its target information and then starts interpreting the target that was called for the build or setup. Once a dependency is reached, Hive should first check the name of the dependency in the project itself. If that fails, Hive checks if the dependency is installed on the system. And finally if that fails, then Hive checks if there is a wrap dependency or if there is an explicit VC repository or source specified.

## Dependencies

Since dependencies import entire projects in, it is necessary to specify the target you wish to import. This target defaults to `lib.hive`. Specifying the name is still required since searching local targets and searching system-installed libraries. If the dependency is a system-installed library, then the target parameter is ignored and the system is checked. If the version parameter is omitted, then Hive searches for the largest version of the installed library on the system. The following is an example of what adding dependencies should look like, it is heavily subject to change.

```
xyz_dep = dependency('XYZ',
    version: '1.0.0', # default: searches for the largest version
    target: 'lib-specific', # default: 'lib'
    repo: 'https://repo.com/xyz.git', # default: <none>
    required: false, # default: true
)
```

After a dependency has been defined, the dependant can now change its exported properties. The dependency is only instantiated at the end of the dependant's definition.

If a target's configuration matches to a previously instantiated configuration, then the dependency should use that prebuilt configuration. If no such prebuilt configuration, then one should be created.

## **.incl** files

Files with the **.incl** extension should be used to clean up target files by spreading out the functionality elsewhere. This would allow the user to split source file adding into its respective directory, like this:

```
# In ./src/src.incl
lib_src += files(
    'example.cpp',
    'bar.cpp',
)

# In ./lib.hive
let lib_src = []
include('src/src.incl')
...
```

Include files will be evaluated within the context of the target they were included in. This means that include files can modify existing variables in the target that included the file.

## Target configuration

The target configuration should be made up of:

- Target type
- Source files
- Include directories
- Compile options (flags)
- Compile definitions
- Linker options (flags) and
- Additional user-defined export variables

Any secondary definition should be fed to one of the above configuration variables or the user-defined export variable.

This would allow for Hive to keep track if the target was already built and, if ever proposed, which target to download from a prebuilt target repository. 

The way a hash would be built is as follows:

- Non-user-defined variables will be placed in their proper and deterministic order
- Every list value will be sorted in alphabetical order
- User-defined variables will be placed at the end of the string and in alphabetical order

This would all happen in-memory and passed over to a hash function that is then compared with existing target hashes. If it matches a target hash, the target wouldn't need to be rebuilt. If it doesn't, the target is built and it's hash is stored in the target metadata.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

The rationale is making everything that could be automated, automated.
