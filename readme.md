**//// Added by Juan Navarro on Feb 17 2025 ////**

### PosgreSQL ODBC driver with GreptimeDB support for Windows



This is a modified version of the PostgreSQL connector to ignore the transaction isolation query
and avoid using repeated NULL bindings in some other queries, this in order to allow
successful connection with a GreptimeDB which uses DataFusion for query optimization 
in the backend.

I modified the ODBC out of necessity since I needed to create a connector that would work with JMP, the data analysis package, and other tools such as excel.

To build this very legacy tool in windows you will need to do a couple things


- You need to download an old, x86 32 bit version of postgresql. Probably the zipped version 9 on postgres binary downloads. Unzip into some folder

- Use `./winbuild/editConfiguration.ps1` to point the build system to the right include, bin, and lib directories in the postgres folder, only the ones for the x86 32 bit postgresql version.

- Install visual studio community, and add any of the MSVC 2015 C++ build tools, its like a 4 gb download. This is so that `dumpbin` exists and the installers can be generated. Mod the dumpbinexe constant in MSProgram-Get.psm1 line 220 to point to your dumpbin, E.G. put  `"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.42.34433\bin\Hostx64\x86\dumpbin.exe"` in there

- Install Wix and necessary extensions
    - dotnet tool install --global wix --version 5.0.2
    - wix extension add --global WixToolset.UI.wixext
    - wix extension add --global WixToolset.Util.wixext

- To build the installers run `.\winbuild\BuildAll.ps1`, once it builds without errors, run `.\installer\buildInstallers.ps1`. The installer will show up under `.\installer\x86\` and `.\installer\x64`

- To use with greptimedb you'll have to use the x64 Unicode driver. An example ODBC connection string is the following:
`Driver={PostgreSQL Unicode};server=localhost;port=4003;database=public;`

**//// Added by Juan Navarro on Feb 17 2025 ////**


/********************************************************************

  PSQLODBC.DLL - A library to talk to the PostgreSQL DBMS using ODBC.


  Copyright (C) 1998          Insight Distribution Systems
  Copyright (C) 1998 - 2024   The PostgreSQL Global Development Group

  Multibyte support was added by Sankyo Unyu Service, (C) 2001.

  The code contained in this library is based on code written by
  Christian Czezatke and Dan McGuirk, (C) 1996.


  This library is free software; you can redistribute it and/or modify
  it under the terms of the GNU Library General Public License as
  published by the Free Software Foundation; either version 2 of the
  License, or (at your option) any later version.

  This library is distributed in the hope that it will be useful, but
  WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
  Library General Public License for more details.

  You should have received a copy of the GNU Library General Public
  License along with this library (see "license.txt"); if not, write to
  the Free Software Foundation, Inc., 675 Mass Ave, Cambridge, MA
  02139, USA.


  How to contact the authors:

  email:   pgsql-odbc@postgresql.org
  website: https://odbc.postgresql.org/


***********************************************************************/
