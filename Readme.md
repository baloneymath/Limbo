#**Limbo Library**
---------

Limbo Library for VLSI CAD Design 

---------

All components are written with C/C++ and API is designed for easy usage and simple embedding. 
Please read this **Readme** file carefully for proper instructions to **install** and **customize**. 

---------

## Components 
| Components              | Languages & Libraries           | Description                                              |
| ----------------------- | ------------------------------- | -------------------------------------------------------- |
| Lef Parser              | flex/bison                      |                                                          |
|                         | boost.spirit                    | deprecated due to efficiency                             |
| Def Parser              | flex/bison                      |                                                          |
|                         | boost.spirit                    | deprecated due to efficiency                             |
| Gds Parser              | stream parser: c/c++            | support .gds and .gds.gz when Boost and Zlib available   |
|                         | ascii parser: boost.spirit      |                                                          |
|                         | gdsdb wrapper: c/c++, boost     | high level wrapper for full layout import/export/flatten |
| Tf Parser               | boost.spirit                    |                                                          |
| Verilog netlist parser  | flex/bison                      | read verilog netlist to initialize nets during placement |
| Lp Parser               | flex/bison                      |                                                          |
| GDF Parser              | flex/bison                      | routing file format from Jason Cong's group at UCLA      |
| Bookshelf Parser        | flex/bison                      | placement file format from UCSD                          |
| LP Min-Cost Flow Solver | boost, lemon                    |                                                          |
| Geometry Algorithms     | c++                             |                                                          |
| String Algorithms       | c++                             |                                                          |
| Math Utilities          | c++                             |                                                          |
| Partition Algorithms    | c++                             |                                                          |
| Placement Algorithms    | c++                             | some common and useful placement algorithms              |
| Graph Algorithms        | c++                             | basic graph algorithms based on Boost.Graph structure    |
| Program Options         | c++                             | package for easy command line options for a program      |
| Makefile Utilities      | makefile                        | makefile help functions                                  |
| Third Party Libraries   | c/c++/fortran                   | OpenBLAS, Csdp, CThreadPool, liblinear, dlx, libdivide   |

## Authors & Maintainers

|  Name              | Affiliation                |  email                            |
| ------------------ | -------------------------- | --------------------------------- |
| Yibo Lin           | ECE Dept, UT Austin        | yibolin@utexas.edu			      |

## Users

|  Name              | Affiliation                |  email                            |
| ------------------ | -------------------------- | --------------------------------- |
| UTDA Folks         | ECE Dept, UT Austin        | http://www.cerc.utexas.edu/utda/  |

## Introduction

### 1. Flex

* A fast scanner generator or lexical analyzer generator. Another famous related software is Lex.
Current parsers support flex 2.5.37. 

### 2. Bison 

* A Yacc-compatible parser generator that is part of the GNU Project. 
	For details, please refer to [here](http://en.wikipedia.org/wiki/GNU_bison).
	Another famous related software is Yacc.

### 3. *(Deprecated)* Boost.Spirit

* An object-oriented, recursive-descent parser and output generation library for C++. 
	For details, please refer to [here](http://www.boost.org/doc/libs/1_55_0/libs/spirit/doc/html/index.html).

### 4. LL Parser 

* In computer science, a LL parser is a top-down parser for a subset of context-free languages. 
	It parses the input from Left to right, performing Leftmost derivation of the sentence. 

### 5. LR Parser 

* In computer science, LR parsers are a type of bottom-up parsers that efficiently handle 
	deterministic context-free languages in guaranteed linear time. 

### 6. LL v.s. LR

* Bison is based on LR parser and Boost.Spirit is based on LL parser.
Please refer to [here](http://cs.stackexchange.com/questions/43/language-theoretic-comparison-of-ll-and-lr-grammars).

### 7. LP and Min-Cost Flow 

* Min-cost flow (MCF) is a special case of linear programs (LP). 
It is usually faster than general LP solver and is also able to achieve integer solution.
Some LP can be directly transformed to MCF problem, and for some other LP, its duality is a MCF problem. 
In this library, an API for lemon (MFC solver) is implemented to solve LP and dual LP problems. 
The API supports reading files with both LGF (lemon graph format) and LP (linear program format) and then dump out solutions. 
Thus, it will be easier to verify results with general LP solvers such as Gurobi or CBC.

## Bug Report 

Please report bugs to Yibo Lin (yibolin at utexas dot edu). 

## Installation

Some components depend on external libraries, such as 

* [Boost](www.boost.org): require BOOST_DIR environment variable to the path where Boost is installed. 
* [Lemon](https://lemon.cs.elte.hu): require LEMON_DIR environment variable to the path where Lemon is installed. 
* [Gurobi](www.gurobi.com): require GUROBI_HOME environment variable to the path where Gurobi is installed. 
* [Flex](http://flex.sourceforge.net): require FLEX_DIR or LEX_INCLUDE_DIR environment variable if the flex version is not 2.5.37. See FAQ for details. 
* [Zlib](http://www.zlib.net) (optional): require ZLIB_DIR environment variable to enable zlib features. If both zlib and boost are available, Gds parser supports reading/writing .gds.gz files. 

Users need to make sure they are properly installed and the corresponding settings are configured. 

### 1. Default installation

* In the directory of limbo library, run
```
make
make test (optional)
make install PREFIX=an_absolute_path
```
The default installation path is the same as building path. 
Please do not set PREFIX with a relative path. 
After installation, it is strongly recommended to export LIMBO_DIR to the path where Limbo library is installed as an environment variable. 

### 2. Customize CXX, CC and FC options 

* CXX sets the compiler for C++ and CC sets the compiler for C. FC sets the fortran compiler which is only needed by some third party packages like OpenBLAS. 
Some examples are as follows, 
```
make CXX=g++ CC=gcc FC=gfortran (default for Linux)
make CXX=clang++ CC=clang FC=gfortran (default for Mac/Darwin)
```

### 3. Customize CXXSTDLIB and CXXSTD options

* CXXSTDLIB is used to control -stdlib=xxx under clang++, and CXXSTD is used to control -std=xxx in most compilers. 

Under clang, following combinations are valid, 
```
make CXXSTDLIB="-stdlib=libstdc++" CXXSTD="-std=c++98" (default)
make CXXSTDLIB="-stdlib=libc++" CXXSTD="-std=c++98"
make CXXSTDLIB="-stdlib=libc++" CXXSTD="-std=c++11"
```

Under gcc, there is no -stdlib option, so following combinations are valid, 
```
make CXXSTD="-std=c++98" (default)
make CXXSTD="-std=c++11"
```

Users must make sure the setting is consistent to dependent libraries during compilation, such as Boost, Lemon, etc. 
For example, if Boost is compiled with "clang++ -stdlib=libstdc++ -std=c++98", the same setting should be used for components dependent to Boost. 

### 4. Customize OPENBLAS options 

* There is a third party OpenBLAS required by some other third party packages, such as Csdp and liblinear. OPENBLAS option is used to control whether compiling these packages. 

If you do not need these packages, set OPENBLAS=0 (default); 
otherwise, set OPENBLAS=1.
The default version of OpenBLAS is not very stable for cross platforms, which often results in compiling errors. 
OpenBLAS is integrated as a submodule which fetch source code from remote repository, so it is always up-to-date version. 
If you already have OpenBLAS in the directory, simply run "git submodule update" to fetch the latest version in the submodule directory. 

## Examples

In the subdirectories of source code, there are test folders including test/example programs.

## FAQ

###1. *(Deprecated)* Compiling errors like
```
LefScanner.cc:5582:21: error: out-of-line definition of 'LexerInput' does not match any declaration in 'LefParserFlexLexer'
                        size_t yyFlexLexer::LexerInput( char* buf, size_t max_size )
```
come from old versions of flex, such as 2.5.35. 

**A:** It can be solved by installing correct flex version 2.5.37 and add the directory to correct flex to PATH environment variable. 

###2. *(Deprecated)* Compiling errors like 
```
LefScanner.cc:3195:8: error: member reference type 'std::istream *' (aka 'basic_istream<char> *') is a pointer; did you mean to use '->'?
                        yyin.rdbuf(std::cin.rdbuf());
```
come from new versions of flex, such as 2.6.0. 

**A:** It can be solved by installing correct flex version 2.5.37 and add the directory to correct flex to PATH environment variable. 

###3. Compiling errors related to LefScanner.cc usually come from the configurations of flex version and environment variables FLEX_DIR and LEX_INCLUDE_DIR. 

**A:** LefScanner.cc needs to include the correct FlexLexer.h from the flex package for compilation; i.e., the version of FlexLexer.h must match the version of the flex executable. 
Most errors for LefScanner.cc are caused by the failure of finding the correct FlexLexer.h (be careful when you have multiple versions of flex installed). 
To solve the problem, users can set the environment variable FLEX_DIR such that $FLEX_DIR/include points to the directory containing FlexLexer.h, or alternatively set LEX_INCLUDE_DIR to the directory containing FlexLexer.h. 
The decision can be made according to how the flex package is installed.  


###4. Crappy linkage error like 
```
"undefined reference to `GdsParser::read(GdsParser::GdsDataBaseKernel&, std::string const&)'" 
```
under gcc 5.1 or later, even though libgdsparser.a is correctly linked. 
**A:** The issue probably comes from potential inconsistent compiling configuration for Limbo and target program due to the new [Dual ABI](https://gcc.gnu.org/onlinedocs/libstdc%2B%2B/manual/using_dual_abi.html) introduced since gcc 5.1. 
The compilation flag _GLIBCXX_USE_CXX11_ABI is used to control whether gcc uses new C++11 ABI, which is turned on in default. 
If Limbo is compiled with C++11 ABI, while target program is compiled with old ABI, then the linkage error appears, vice versa. 
Therefore, it is necessary to make sure the same STL ABI is used for compiling Limbo and target program. 
In other words, set consistent _GLIBCXX_USE_CXX11_ABI values. 
A safe way is to leave it to the default value. 

## Copyright 
The software is released under MIT license except third party packages. Please see the LICENSE file for details. 

Third party package **c-thread-pool** is released under MIT license. 

Third party package **csdp** is released under CPL v1.0 license. 

Third party package **OpenBLAS** has its copyright reserved; please check its license. 

Third party package **liblinear** has its copyright reserved; please check its license.

## Tutorial

Some components in limbo library do not need linkage, so they can be used directly by including the headers, while some components require linkage to the corresponding static libraries.  
Here are some simple example to show the basic usage and compiling commands with gcc under Linux. 
For clang, the compiling commands are slightly different as users need to specify the same **-stdlib** as that in **CXXSTDLIB** flag used to install the library. 

###1. Compare two strings case-insensitive 

Source code: compare.cpp
```
#include <iostream>
#include <string>
#include <limbo/string/String.h>

int main()
{
    std::string s1 = "limbo2343slimbo";
    std::string s2 = "LiMbo2343SliMbo";

    if (s1 == s2)
        std::cout << "string " << s1 << " and " << s2 << " is equal case-sensitive\n";
    else 
        std::cout << "string " << s1 << " and " << s2 << " is not equal case-sensitive\n";

    if (limbo::iequals(s1, s2))
        std::cout << "string " << s1 << " and " << s2 << " is equal case-insensitive\n";
    else 
        std::cout << "string " << s1 << " and " << s2 << " is not equal case-insensitive\n";

	return 0;
}
```
Compiling and running commands (assuming LIMBO_DIR is exported as the environment variable to the path where limbo library is installed)
```
g++ compare.cpp -I $LIMBO_DIR/include
./a.out
```
Output 
```
string limbo2343slimbo and LiMbo2343SliMbo is not equal case-sensitive
string limbo2343slimbo and LiMbo2343SliMbo is equal case-insensitive
```

###2. Basic usage of Limbo.ProgramOptions component

Limbo.ProgramOptions provides easy API for developers to define command line options to their programs with a highly extendable manner, i.e., only single line of code for each option. No need to write any code for parsing. 
It also offers detailed error reporting scheme that print incorrect and missing arguments. 

Source code: ProgramOptionsExample.cpp (see limbo/test/programoptions for details)
```
#include <iostream>
#include <string>
#include <vector>
#include <limbo/programoptions/ProgramOptions.h>

int main(int argc, char** argv)
{
    bool help = false;
    int i = 0;
    double fp = 0;
    std::vector<int> vInteger;

    typedef limbo::programoptions::ProgramOptions po_type;
    using limbo::programoptions::Value;
    po_type desc ("My options");
    // add user-defined options
    desc.add_option(Value<bool>("-help", &help, "print help message").toggle(true).default_value(false).toggle_value(true).help(true)) // specify help option 
        .add_option(Value<int>("-i", &i, "an integer").default_value(100, "1.0.0"))
        .add_option(Value<double>("-f", &fp, "a floating point").required(true)) // the floating point option is a required option, so user must provide it 
        .add_option(Value<std::vector<int> >("-vi", &vInteger, "vector of integers"))
        ;

    try 
    {
        bool flag = desc.parse(argc, argv);
        if (flag) 
            std::cout << "parsing succeeded\n";
    }
    catch (std::exception& e)
    {
        std::cout << "parsing failed\n";
        std::cout << e.what() << "\n";
    }

    // print help message 
    if (help)
    {
        std::cout << desc << "\n";
        return 1; 
    }

    std::cout << "help = " << ((help)? "true" : "false") << std::endl;
    std::cout << "i = " << i << std::endl;
    std::cout << "fp = " << fp << std::endl;
    std::cout << "vInteger = ";
    for (std::vector<int>::const_iterator it = vInteger.begin(); it != vInteger.end(); ++it)
        std::cout << *it << " ";
    std::cout << std::endl;

    return 0;
}
```
Compiling and running commands (assuming LIMBO_DIR is valid and limbo library has been properly installed)
```
# linkage is necessary for Limbo.ProgramOptions
g++ ProgramOptionsExample.cpp -I $LIMBO_DIR/include -L $LIMBO_DIR/lib -lprogramoptions
# test help message 
./a.out -help
# test integer and floating point number 
./a.out -i 20 -f 1.5 
# test vector of integers; the numbers are appending to the container 
./a.out -i 20 -f 1.5 -vi 10 -vi 30 -vi 50
```
Output
```
parsing succeeded
help = true
i = 100
fp = 0
vInteger =
```
```
parsing succeeded
help = false
i = 20
fp = 1.5
vInteger =
```
```
parsing succeeded
help = false
i = 20
fp = 1.5
vInteger = 10 30 50
```

###3. Basic usage of Limbo.Gdsii component

Limbo.Gdsii consists of Gds parser and Gds database, which provides reading and writing in GDSII format as well as easy API for IO and flattening of full layout with hierarchies. 
More specifically, it supports IO for .gds files and compressed .gds files (i.e., .gds.gz, supported by Boost.Iostreams and ZLIB). 
Whether a gds file is compressed or not is detected by file name. That is, a file ending with .gz is regarded as compressed. 

Source code: GdsReaderExample.cpp (see limbo/test/parsers/gdsii for details)
```
#include <iostream>
#include <limbo/parsers/gdsii/stream/GdsReader.h>

/// test enum callbacks
struct EnumDataBase : public GdsParser::GdsDataBaseKernel
{
	EnumDataBase()
	{
		std::cout << "constructing EnumDataBase" << std::endl;
	}
	///////////////////// required callbacks /////////////////////
	virtual void bit_array_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, vector<int> const& vBitArray)
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, vBitArray);
	}
	virtual void integer_2_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, vector<int> const& vInteger)
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, vInteger);
	}
	virtual void integer_4_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, vector<int> const& vInteger)
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, vInteger);
	}
	virtual void real_4_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, vector<double> const& vFloat) 
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, vFloat);
	}
	virtual void real_8_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, vector<double> const& vFloat) 
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, vFloat);
	}
	virtual void string_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, string const& str) 
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, data_type, str);
	}
	virtual void begin_end_cbk(GdsParser::GdsRecords::EnumType record_type)
	{
		std::cout << __func__ << std::endl;
		this->general_cbk(record_type, GdsParser::GdsData::NO_DATA, vector<int>(0));
	}

    /// this is not the most efficient solution
    /// but it is concise since we wrap everything in a template function 
	template <typename ContainerType>
	void general_cbk(GdsParser::GdsRecords::EnumType record_type, GdsParser::GdsData::EnumType data_type, ContainerType const& data)
	{
		std::cout << "ascii_record_type: " << GdsParser::gds_record_ascii(record_type) << std::endl
			<< "ascii_data_type: " << GdsParser::gds_data_ascii(data_type) << std::endl 
			<< "data size: " << data.size() << std::endl;
        switch (record_type)
        {
            case GdsParser::GdsRecords::UNITS:
                break;
            case GdsParser::GdsRecords::BOUNDARY:
                break;
            case GdsParser::GdsRecords::LAYER:
                std::cout << "LAYER = " << data[0] <<  std::endl;
                break;
            case GdsParser::GdsRecords::XY:
                for (typename ContainerType::const_iterator it = data.begin(); it != data.end(); ++it)
                    std::cout << *it << " "; 
                std::cout << std::endl; 
                std::cout << data.size() << std::endl;
                break;
            case GdsParser::GdsRecords::ENDEL:
                break;
            default:
                break;
        }
	}
};

int main(int argc, char** argv)
{
	if (argc > 1)
    {
        EnumDataBase edb;
		std::cout << "test enum api\n" << GdsParser::read(edb, argv[1]) << std::endl;
    }
	else std::cout << "at least 1 argument is required" << std::endl;

	return 0;
}
```
Compiling and running commands (assuming LIMBO_DIR is valid and limbo library has been properly installed)
```
# linkage is necessary for Limbo.GdsParser
# if BOOST_DIR and ZLIB_DIR are defined when building Limbo (.gds.gz supported)
g++ GdsReaderExample.cpp -I $LIMBO_DIR/include -L $LIMBO_DIR/lib -lgdsparser -L $BOOST_DIR/lib -lboost_iostreams -L $ZLIB_DIR -lz
# else if BOOST_DIR and ZLIB_DIR are NOT defined when building Limbo (.gds.gz not supported)
g++ GdsReaderExample.cpp -I $LIMBO_DIR/include -L $LIMBO_DIR/lib -lgdsparser 
# test a simple gds file 
./a.out benchmarks/test_reader.gds
# test a simple gds.gz file (need BOOST_DIR and ZLIB_DIR defined when building Limbo)
./a.out benchmarks/test_reader_gz.gds.gz
```
Output
```
constructing EnumDataBase
integer_2_cbk
ascii_record_type: HEADER
ascii_data_type: INTEGER_2
data size: 1
integer_2_cbk
ascii_record_type: BGNLIB
ascii_data_type: INTEGER_2
data size: 12
string_cbk
ascii_record_type: LIBNAME
ascii_data_type: STRING
data size: 6
real_8_cbk
ascii_record_type: UNITS
ascii_data_type: REAL_8
data size: 2
integer_2_cbk
ascii_record_type: BGNSTR
ascii_data_type: INTEGER_2
data size: 12
string_cbk
ascii_record_type: STRNAME
ascii_data_type: STRING
data size: 7
begin_end_cbk
ascii_record_type: BOUNDARY
ascii_data_type: NO_DATA
data size: 0
integer_2_cbk
ascii_record_type: LAYER
ascii_data_type: INTEGER_2
data size: 1
LAYER = 10002
integer_2_cbk
ascii_record_type: DATATYPE
ascii_data_type: INTEGER_2
data size: 1
integer_4_cbk
ascii_record_type: XY
ascii_data_type: INTEGER_4
data size: 14
19523 234260 20000 234260 20000 234356 19924 234356 19924 234292 19523 234292 19523 234260 
14
begin_end_cbk
ascii_record_type: ENDEL
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: BOUNDARY
ascii_data_type: NO_DATA
data size: 0
integer_2_cbk
ascii_record_type: LAYER
ascii_data_type: INTEGER_2
data size: 1
LAYER = 10002
integer_2_cbk
ascii_record_type: DATATYPE
ascii_data_type: INTEGER_2
data size: 1
integer_4_cbk
ascii_record_type: XY
ascii_data_type: INTEGER_4
data size: 10
0 100000 1018 100000 1018 125000 0 125000 0 100000 
10
begin_end_cbk
ascii_record_type: ENDEL
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: ENDSTR
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: ENDLIB
ascii_data_type: NO_DATA
data size: 0
test enum api
1
```
```
constructing EnumDataBase
integer_2_cbk
ascii_record_type: HEADER
ascii_data_type: INTEGER_2
data size: 1
integer_2_cbk
ascii_record_type: BGNLIB
ascii_data_type: INTEGER_2
data size: 12
string_cbk
ascii_record_type: LIBNAME
ascii_data_type: STRING
data size: 6
real_8_cbk
ascii_record_type: UNITS
ascii_data_type: REAL_8
data size: 2
integer_2_cbk
ascii_record_type: BGNSTR
ascii_data_type: INTEGER_2
data size: 12
string_cbk
ascii_record_type: STRNAME
ascii_data_type: STRING
data size: 7
begin_end_cbk
ascii_record_type: BOUNDARY
ascii_data_type: NO_DATA
data size: 0
integer_2_cbk
ascii_record_type: LAYER
ascii_data_type: INTEGER_2
data size: 1
LAYER = 10002
integer_2_cbk
ascii_record_type: DATATYPE
ascii_data_type: INTEGER_2
data size: 1
integer_4_cbk
ascii_record_type: XY
ascii_data_type: INTEGER_4
data size: 14
19523 234260 20000 234260 20000 234356 19924 234356 19924 234292 19523 234292 19523 234260 
14
begin_end_cbk
ascii_record_type: ENDEL
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: BOUNDARY
ascii_data_type: NO_DATA
data size: 0
integer_2_cbk
ascii_record_type: LAYER
ascii_data_type: INTEGER_2
data size: 1
LAYER = 10002
integer_2_cbk
ascii_record_type: DATATYPE
ascii_data_type: INTEGER_2
data size: 1
integer_4_cbk
ascii_record_type: XY
ascii_data_type: INTEGER_4
data size: 10
0 100000 1018 100000 1018 125000 0 125000 0 100000 
10
begin_end_cbk
ascii_record_type: ENDEL
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: ENDSTR
ascii_data_type: NO_DATA
data size: 0
begin_end_cbk
ascii_record_type: ENDLIB
ascii_data_type: NO_DATA
data size: 0
test enum api
1
```
