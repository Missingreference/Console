# Console
A developer console implementation. Submit commands to the console that affect the runtime of the application. Receive output from the console to receive debugging information or information on the state of the runtime application.

The core implementation of the Developer Console is seperate from the visual representation of the console. For example, the visual representation of the console has to be implemented specifically for Unity. As it stands thread safety is not guarenteed and fixes will be implemented as needed. Pull requests welcome.

The main functions to use are Console.Log, Console.LogWarning and Console.LogError. You can also use Console.Print, Console.PrintWarning, Console.PrintError that does not append NewLine to each entry.

You can receive the output of the console by subscribing to the events Console.onOutputLog, Console.onOutputWarning, Console.onOutputError. Commands are registered using Reflection by iterating through AppDomain.CurrentDomain.GetAssemblies(). For whatever reason if you need to you can reload all commands using Console.ReloadCommands.

ConsoleCommand is the base class for all commands and must have implemented name and helpMessage properties as well as the Execute function. Some basic commands include:
print - Output text to the console.
help - Output the help message of the specified command name to the console.
list - Output all available and loaded commands to the console.
Other commands such as 'clear' or 'quit' cannot be implemented by the core of the console due to not having a visual representation of the console itself or affect the state of the application and must be implemented based on the application it is being built for. For example, these commands would be implemented in the Unity version of the implementation.

### Unity
This implementation uses Unity's UGUI and TextMeshPro to create the UI of the ConsoleUI. This UI will color the text based on the type of log outputted from the console. The UI was created to look and function very similar to Valve's Source engine developer console(https://developer.valvesoftware.com/wiki/Developer_Console) including an adjustable window and performant TextArea that shows the outputted text.

UnityCommand as an abstract class will execute the commands during an early execution order point during the Update loop that way commands are executed on Unity's main thread and at a predictable time of the execution of the game. Since commands like 'clear' and 'quit' cannot be implemented in the core of the Console, 'clear' will remove all text from the TextArea and 'quit' will call UnityEngine.Application.Quit() implemented each as a UnityCommand.
