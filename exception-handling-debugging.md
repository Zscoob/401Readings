# 401Readings
Readings from 401
## Exception Handling and debugging 
### try/catch blocks
Use "try" block for codes that may throw excepeption.
statements used to handle exception in "catch" blocks.

using System;
using System.IO;

public class ProcessFile
{
    public static void Main()
    {
        try
        {
            using (StreamReader sr = File.OpenText("data.txt"))
            {
                Console.WriteLine($"The first line of this file is {sr.ReadLine()}");
            }
        }
        catch (FileNotFoundException e)
        {
            Console.WriteLine($"The file was not found: '{e}'");
        }
        catch (DirectoryNotFoundException e)
        {
            Console.WriteLine($"The directory was not found: '{e}'");
        }
        catch (IOException e)
        {
            Console.WriteLine($"The file could not be opened: '{e}'");
        }
    }
}

CLR  catches not handled by catch, instead:
  -debug box
  -stops executing with prompt
  -error prints

### Statement Keywords
Selection statemenst:
  - if
  - else
  - switch
  - case
Iteration statemenst:
  - do
  - for
  - foreach
  - in
  - while
Jump statmenets:
  - break
  - continue
  - default
  - goto
  - return
  - yeild
Exception handling statemenst:
  - throw
  - try-catch
  - try-finally
  - try-catch-finally
Checked and unchecked
  - checked
  - unchecked
Fixed statement
  - fixed
Lock statmenet
  - lock

### Thereac-25
Therack-25 was a computer-controlled radiation therapy machine made by AECL
Overdosed patients with radiation on 6 occasions between 1985-1987 due to prog errors
overconfidance causes bugs and in this case injury and death
"The six occasions occurred when the high-current electron beam generated in X-ray mode was delivered directly to patients. Two software faults were to blame."
User error, tech selected x-ray before switching to electron mode, electron beam set for xray with no target...another instance, electron beam to activate during field-light mode, no beam scanner was active.

Main causes:
  - software impossible to test in a clean automated way
  - AECL did not have the software code independently reviewed
  - AECL did not consider reliability modeling and risk management
  - The system noticed that something was wrong and halted the X-ray beam, but merely displayed the word "MALFUNCTION" followed by a number from 1 to 64. The user manual did not explain or even address the error codes, so the operator pressed the P key to override the warning and proceed anyway
  - AECL personnel, as well as machine operators, initially did not believe complaints. This was likely due to overconfidence
  - AECL had never tested the Therac-25 with the combination of software and hardware until it was assembled at the hospital
  - The failure occurred only when a particular nonstandard sequence of keystrokes was entered on the terminal which controlled the computer: an "X" to (erroneously) select 25 MeV photon mode followed by "cursor up", "E" to (correctly) select 25 MeV Electron mode, then "Enter", all within eight seconds
  - The design did not have any hardware interlocks to prevent the electron-beam from operating in its high-energy mode without the target in place
  - The engineer had reused software from older models
  - The hardware provided no way for the software to verify that sensors were working correctly
  - The equipment control task did not properly synchronize with the operator interface task
  - The software set a flag variable by incrementing it, rather than by setting it to a fixed non-zero value. Occasionally an arithmetic overflow occurred, causing the flag to return to zero and the software to bypass safety checks

### Ariane 5
Ariane 5 is a European heavy-lift launch vehicle that is part of the Ariane rocket family
It is used to deliver payloads into geostationary transfer orbit (GTO) or low Earth orbit (LEO).
Ariane 5 has a commonly used dual-launch capability where up to two large geostationary belt communication satellites can be mounted using a SYLDA carrier 
Ariane 5 is operated and marketed by Arianespace as part of the Ariane programme
Ariane 5 has been refined since the first launch in successive versions, "G", "G+", "GS", "ECA", and most recently, "ES". ESA originally designed Ariane 5 to launch the Hermes spaceplane, and thus intended it to be human rated from the beginning.
Ariane 5 has a commonly used dual-launch capability where up to two large geostationary belt communication satellites can be mounted using a SYLDA carrier
Up to eight secondary payloads, usually small experiment packages or minisatellites, can be carried with an ASAP platform

### C# 7.0 in a Nutshell: Try Statements and Exceptions
A try statement specifies a code block which is subject to error-handling.  the try block must be followed by a catch block 

try
{
... // exception may get thrown within execution of this block
}
catch (ExceptionA ex)
{
... // handle exception of type ExceptionA
}
catch (ExceptionB ex)
{
... // handle exception of type ExceptionB
}
finally
{
... // cleanup code
}

When an exception is thrown the CLR performs a test. if the test fails an error box pops and the program quits

catch clauses specify what exception to catch
  - System.Exception or a subclass of the latter.

exception filters catch clauses by adding a when clause.

catch (WebException ex) when (ex.Status == WebExceptionStatus.Timeout)
{
...
}

A finally block always executes weather or not an exception is thrown.
typically used for cleanup code
  - after catch block finishes
  - after control leaves the try block due to jump statements
  - after try block ends
Infinite loops null finally blocks

the using statement encapsulate unmerged resources 
provides a simplified syntax for Dispose in the following block example:

using (StreamReader reader = File.OpenText ("file.txt"))
{
...
}

is precisely equivalent to:

{
StreamReader reader = File.OpenText ("file.txt");
try
{
...
}
finally
{
if (reader != null)
((IDisposable)reader).Dispose();
}
}


Exceptions can be thrown either by the runtime or in user code

Prior to C# 7, throw was always a statement. Now it can also appear as an expression
in expression-bodied functions:

public string Foo() => throw new NotImplementedException();

A throw expression can also appear in a ternary conditional expression:

string ProperCase (string value) =>
value == null ? throw new ArgumentException ("value") :
value == "" ? "" :
char.ToUpper (value[0]) + value.Substring (1);

You can capture and rethrow an exception as follows:
try { ... }
catch (Exception ex)
{
// Log error
...
throw; // Rethrow same exception
}

The most important properties of System.Exception are the following:
StackTrace
A string representing all the methods that are called from the origin of the
exception to the catch block.
Message
A string with a description of the error.
InnerException
The inner exception (if any) that caused the outer exception. This, itself, may
have another InnerException.