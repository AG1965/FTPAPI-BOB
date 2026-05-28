# BOB, start with Phase 1.1

That's the Migration to Free-Format RPG

He created a [MODERNIZATION_README.md](/MODERNIZATION_README.md) file.

### ad "Code Quality" - "Column indicators":

It seems as if he thinks the "B01" and "E01" in columns 1 to 3 of the original file (see lines 55 and 60 of [TESTGET.MBR](/QRPGLESRC.FILE/TESTGET.MBR)) are "column indictors", which they aren't, they just mark the Begin and the End of code enclosed by IF and ENDIF.
But this misunderstandig doesn't have consequences for the converted code.

## Note to BOB's mothers and dads:

The conversion of fixed format to **FREE should ignore everything in columns 1 to 5, where people put all kinds of characters, including 5250 color attributes (which may appear in comments in nearly any position!).

Ideally, the program that copied the source file members to the "IFS" had already removed all that now (in the best case) useless stuff.

## Continue

Still sharing my screen, i wanted BOB to continue, modernizing the code itself.  
"mach weiter mit der Modernisierung selber, dem Code"

He understood, examined the files [TESTMGET.MBR](/QRPGLESRC.FILE/TESTMGET.MBR) and [TESTURL.MBR](/QRPGLESRC.FILE/TESTURL.MBR) and said:
]