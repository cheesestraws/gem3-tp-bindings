gem3-tp-bindings
================

This is old stuff, but I keep losing it, so I'm putting it here so that

1. I won't lose it any more, and
2. People who still use FreeGEM can use the bindings.

These are cleaned-up versions of the original Digital Research GEM/3
Turbo Pascal bindings from 1987, so that they work with Turbo Pascal
versions newer than version 5.  This tidied version was originally released
in 2001.

Source code which used the original bindings will require only small
alterations:

* The original bindings used a record type called OBJECT for user interface
  objects.  Since TP5, "object" is a reserved word, so the type has been
  renamed "UIOBJECT".
* The intr() and msdos() calls used to take a record of a user-defined type.
  Since TP5, the calls take a predefined type, called 'registers'.  Changing
  this required some of the variable names to be changed, from 'registers' to
  '_registers' to avoid the type and variable names clashing. 
* All of the include files have been bundled up into one unit to make the whole
  thing easier to use.
  
The original files were:

* AESBIND.INC
* C2PASCAL.INC
* DEBUG.INC
* DOSBIND.INC
* MACHINE.INC
* OBDEF.INC
* VDIBIND.INC
* VDIDEF.INC


Things which never got done but ought to have
---------------------------------------------

* Fix functions which return ^UIOBJECTs so that they actually return that
  type, rather than a pointer to the first byte, saving a messy typecast.
* Add support for the FreeGEM gubbins.


License
-------

This software was, I believe, part of the Caldera GEM dump.  Therefore, it
is under the GPLv2 (see LICENSE file)


Original headers, authors and version histories
-----------------------------------------------

The original headers from the files follow:

    (*****************************************************************)
    (**      File Name     :  AESBIND.PAS                           **)
    (**                                                             **)
    (**      Turbo Pascal bindings for GEM AES                      **)
    (**                                                             **)
    (**      Comments :                                             **)
    (**        - it is assumed that C2PASCAL.INC is included        **)
    (**            before this file                                 **)
    (**        - All arrays are relative to zero to match the       **)
    (**            Programmer's Guide (GEM AES VOL 2)               **)
    (**        - All strings dealt with at this level are assumed   **)
    (**            to be in PASCAL format (instead of passing       **)
    (**            addresses, untyped variables are passed, set     **)
    (**            equal to MaxString variables, and converted     **)
    (**            locally to 'C' format strings (see C2PASCAL.INC) **)
    (**                                                             **)
    (**      History :                                              **)
    (**        Original code was for Pascal-MT+ AES v1.0            **)
    (**          written by Athol M. Foden at Digital Research Inc  **)
    (**          in February 1985                                   **)
    (**        code that was in files GEMATYP.I, GEMPAVAR.I, and    **)
    (**          AESBND.PAS is included in this file                **)
    (**                                                             **)
    (**        modified for Turbo Pascal and updated to work with   **)
    (**          GEM version 2.0 by Mark Lewis at Federal Aviation  **)
    (**          Administration in November 1986                    **)
    (**        revised code was called GEMBIND.PAS                  **)
    (**                                                             **)
    (**        present code is cleaned up version of GEMBIND.PAS    **)
    (**          incorporating LONG record type (see C2PASCAL.INC)  **)
    (**        revised by Sally Sheridan at FAA, Feb 1987           **)
    (**                                                             **)
    (**                                                             **)
    (*****************************************************************)


    (************************************************************************)
    (*      c2pascal.Inc                                                    *)
    (*      Include file for types and procedures used in 'C'               *)
    (*        for Turbo PASCAL on 8086 machine with MS DOS                  *)
    (************************************************************************)
    (**********************************************************************)
    (*                                                                    *)
    (*      Convert Pascal style strings to/from C style strings.         *)
    (*        Pascal strings - byte 0 contains length of string           *)
    (*        C strings - end of string is indicated by a byte of zeros   *)
    (*                                                                    *)
    (*                                                                    *)
    (*      NOTE:                                                         *)
    (*        untyped variable parameters are used so strings can be      *)
    (*          appropriate lengths in the applications program           *)
    (*                                                                    *)
    (*        the variable parameter strings may contain up to 255 chars  *)
    (*                                                                    *)
    (*                                                                    *)
    (*      Last modified: 17 March 1987 by Sally Sheridan, FAA            *)
    (*                                                                    *)
    (**********************************************************************)

    (*****************************************************************)
    (**      File Name     :  DEBUG.INC                             **)
    (**                                                             **)
    (**      Turbo Pascal procedures to help with debugging GEM     **)
    (**        programs                                             **)
    (**                                                             **)
    (**      Comments :                                             **)
    (**        - it is assumed that C2PASCAL.INC, AESBIND.INC, and  **)
    (**            VDIDEF.INC are included before this file         **)
    (**        - All arrays are relative to zero to match the       **)
    (**            Programmer's Guide (GEM AES VOL 2)               **)
    (**        - All strings dealt with at this level are assumed   **)
    (**            to be in PASCAL format                           **)
    (**        - untyped variable parameters are used, and set      **)
    (**            equal to CharStrings locally, so any size string **)
    (**            may be used in the application program           **)
    (**                                                             **)
    (**      History :                                              **)
    (**        written by Sally Sheridan at FAA, Jan 1987           **)
    (**                                                             **)
    (*****************************************************************)

    (**********************************************************************)
    (**     File : DOSBIND.INC                                           **)
    (**                                                                  **)
    (**     procedures for interfacing application programs with DOS     **)
    (**                                                                  **)
    (**     History :                                                    **)
    (**       original code was DOSBIND.C and DOSASM.ASM                 **)
    (**       written by Lee Lorenzen, Digital Research Inc, 2/15/85     **)
    (**                                                                  **)
    (**       translated to Turbo Pascal for PC-DOS                      **)
    (**       by Sally Sheridan, FAA, February 1987                      **)
    (**                                                                  **)
    (**     NOTE:                                                        **)
    (**       the file C2PASCAL.INC must be included before this file    **)
    (**       to define LONG data structure and handle string conversion **)
    (**                                                                  **)
    (**       UNTYPED variable parameters are used in some routines      **)
    (**                                                                  **)
    (**********************************************************************)

    (*****************************************************************)
    (**      File Name     :  MACHINE.INC                           **)
    (**                                                             **)
    (**      machine and language specific functions and procedures **)
    (**        these are for Turbo PASCAL on an 8086 machine        **)
    (**                                                             **)
    (**      Comments :                                             **)
    (**        - it is assumed that C2PASCAL.INC and DEBUG.INC are  **)
    (**            before this file                                 **)
    (**        - all functions prefixed with LL- return ADDRESS     **)
    (**            type values                                      **)
    (**                                                             **)
    (**      History :                                              **)
    (**        Original code was in C                               **)
    (**          written by Lee Lorenzen at Digital Research Inc    **)
    (**          09/29/84 to 02/08/85                               **)
    (**          as file MACHINE.H                                  **)
    (**                                                             **)
    (**        modified for Turbo Pascal and updated to work with   **)
    (**          GEM version 2.0 by Sally Sheridan at Federal       **)
    (**          Aviation Administration in December 1986           **)
    (**          revised code was part of OBDEFS.I                  **)
    (**                                                             **)
    (**        addition of math functions to simulate C-type        **)
    (**          variables (unsigned words and longs) added by      **)
    (**          Sally Sheridan at FAA, March 1987                  **)
    (**          and separated from OBDEFS.I as MACHINE.INC         **)
    (**                                                             **)
    (**                                                             **)
    (*****************************************************************)

    {      OBDEFS.I                03/15/84 - 02/08/85     Gregg Morris    }
    {      GEM DEVELOPER KIT       06/07/86                Lowell Webster  }
    {      TURBO PASCAL            11/14/86                Mark Lewis      }
    {      TURBO PASCAL extensions  3/87                   Sally Sheridan  }

    (*****************************************************************)
    (**      File Name     :  VDIBIND.INC                           **)
    (**                                                             **)
    (**      Turbo Pascal bindings for GEM VDI                      **)
    (**                                                             **)
    (**      Comments :                                             **)
    (**        - it is assumed that C2PASCAL.INC, AESBIND.INC, and  **)
    (**            VDIDEF.INC are included before this file         **)
    (**            DEBUG.INC may also be included before it         **)
    (**        - All arrays are relative to zero to match the       **)
    (**            Programmer's Guide (GEM VDI VOL 1)               **)
    (**        - All strings dealt with at this level are assumed   **)
    (**            to be in PASCAL format and are converted to      **)
    (**            or from C format at this level (see procedures   **)
    (**            in C2PASCAL.INC)                                 **)
    (**                                                             **)
    (**      History :                                              **)
    (**        Original code was for Pascal-MT+ and VDI v1.0        **)
    (**          written by Athol M. Foden at Digital Research Inc  **)
    (**          in February 1985                                   **)
    (**                                                             **)
    (**        modified for Turbo Pascal and updated to work with   **)
    (**          GEM version 2.0 by Mark Lewis at Federal Aviation  **)
    (**          Administration in December 1986                    **)
    (**        revised code was called VDIBIND.PAS                  **)
    (**                                                             **)
    (**        present code is cleaned up version of VDIBIND.PAS    **)
    (**          incorporating LONG record type and translation of  **)
    (**          strings within these procedures and functions      **)
    (**        revised by Sally Sheridan at FAA, Feb 1987           **)
    (**                                                             **)
    (**                                                             **)
    (*****************************************************************)

    (*****************************************************************)
    (**      File Name     :  VDIDEF.INC                            **)
    (**                                                             **)
    (**      Turbo Pascal definitions and constants required by     **)
    (**        VDIBIND.INC                                          **)
    (**                                                             **)
    (**      Comments :                                             **)
    (**        - C2PASCAL.INC, and AESBIND.INC may be included      **)
    (**            before this file                                 **)
    (**        - All arrays are relative to zero to match the       **)
    (**            Programmer's Guide (GEM VDI VOL 1)               **)
    (**        - All strings dealt with at this level are assumed   **)
    (**            to be in PASCAL format                           **)
    (**                                                             **)
    (**      History :                                              **)
    (**        Original code was for Pascal-MT+ and VDI v1.0        **)
    (**          written by Athol M. Foden at Digital Research Inc  **)
    (**          in February 1985                                   **)
    (**        original code was in files GEMPCON.I, GEMPTYPE.I,    **)
    (**          and GEMPVAR.I                                      **)
    (**                                                             **)
    (**        modified for Turbo Pascal and updated to work with   **)
    (**          GEM version 2.0 by Mark Lewis at Federal Aviation  **)
    (**          Administration in December 1986                    **)
    (**        revised code was called GEMSTD.PAS                   **)
    (**                                                             **)
    (**        present code is cleaned up version of GEMSTD.PAS     **)
    (**          incorporating LONG record type                     **)
    (**        revised by Sally Sheridan at FAA, Feb 1987           **)
    (**                                                             **)
    (*****************************************************************)
