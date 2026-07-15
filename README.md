# PyBroma

> [!CAUTION]
> This library is currently highly experimental and features, APIs, or bindings may break without prior notice at any time. Use at your own risk in production-critical software!

A Python wrapper for [Broma](https://github.com/geode-sdk/broma), designed to parse `.bro` files from the [Geometry Dash Geode bindings](https://github.com/geode-sdk/bindings).

## Features

- **Fast Prototyping:** Avoid slow C++ compile times with a scriptable Python environment.
- **Mod Automation:** Generate code and automate reverse-engineering workflows for Geometry Dash and Geode mods.
- **Tooling Support:** Ideal for creating tool-specific headers (e.g. Ghidra/IDA header files like in [BromaIDA](https://github.com/Stazzical/BromaIDA)).

## How To Use

```python
from pybroma import Root


# test.bro:
# class BindedClass {
#     void bindedFunction() = mac 0xd5db0, win 0x3c8d, ios 0xa83bc;

#     int m_member1;
#     int m_member2;
# }

# class OtherBindedClass : BindedClass {
#     virtual void otherBindedFunction() = mac 0x7e3bc, win 0x5a1c, ios 0x8e412;

#     // win and ios addresses have not been found yet, will not generate
#     static int staticFunction(int a, bool c) = mac 0x74bd3;

#     // Embed c++ code
#     inline int getIndex(int index) {
#         return m_myVector[index];
#     } 

#     // templates supported
#     std::vector<int> m_myVector;
# }

root = Root("test.bro")
for c in root.classes:
    for f in c.fields:
        # NOTE: functions that aren't a Function Bind Field do not return...
        if func := f.getAsFunctionBindField():
            # This will make a dictionary mainly to make iteration a bit easier...
            print(func.prototype.args)
# output        
# {}
# {}
# {'a': <pybroma.PyBroma.Type object at 0x0000018C6D507B70>, 'c': <pybroma.PyBroma.Type object at 0x0000018C6D507B30>}
```

## Installation

### Option 1: Install via Prebuilt Wheels

Download the compatible wheel for your OS, architecture, and Python version from the [GitHub Releases](https://github.com/prevter/PyBroma/releases) page.

### Option 2: Install directly from Git

Run the following command in your terminal:

```bash
pip install git+https://github.com/prevter/PyBroma
```

## Frequently Asked Questions

### Will this be released on PyPI?

Due to licensing and redistribution limitations regarding the original codebase, an official PyPI release is unlikely. This repository serves as a maintained fork and functions as the primary backend for [BromaIDA](https://github.com/Stazzical/BromaIDA).

### Will Geode officially adopt this library?

There are no current plans for official adoption. If you find the tool useful for Geode development, feel free to recommend it to others in the community.

## Bugs and Issues

- No known issues at this time. If you get any errors while using PyBroma or scripts using it (e.g. BromaIDA), make sure you have installed the latest version available.

## TODOs

- [ ] Broma Writer/Formatter
- [ ] Class member injector (For helping with pull requests to the bindings)
