
# CodeToSurvive: A Gamified Programming Survival Simulator

<image-card alt="CodeToSurvive City Map Banner" src="URL_PLACEHOLDER_1" ></image-card>
<img width="626" height="418" alt="image" src="https://github.com/user-attachments/assets/42d32ff1-b726-4fef-8ed5-27e075234374" />
<img width="621" height="413" alt="image" src="https://github.com/user-attachments/assets/f9d9cedb-2314-467a-91ee-ad1c8d81bdc5" />


## 1. Executive Overview

CodeToSurvive is a complex, desktop-based RPG simulator engineered in C# (.NET 8.0) and WPF that transforms theoretical computer science education into an interactive resource-management ecosystem. Traditional educational pathways often fail to bridge the gap between abstract Object-Oriented paradigms and real-world application, resulting in knowledge attrition and disengagement.

This platform solves that challenge by placing the user inside an event-driven virtual economy. By implementing a custom-built lexical analyzer and parser to validate player-submitted code, integrating real-time collision detection on a dynamic canvas, and persisting complex relational game states via Entity Framework Core, CodeToSurvive stands as a comprehensive demonstration of full-stack desktop software engineering. It demands that players balance coding proficiency with financial and energy resource management, simulating the actual pressures of the tech industry.

## 2. Core Architectural Framework

```text
       [ Keyboard Input Events (W,A,S,D) ]
                     │
                     ▼
     ┌───────────────────────────────┐
     │  1. EVENT-DRIVEN GAME LOOP    │ ──► [Collision Engine] ──► Building Interaction
     └───────────────────────────────┘
                     │
                     ▼ [Context Switch]
     ┌───────────────────────────────┐
     │ 2. CUSTOM AST CODE PARSER     │ ──► Analyzes Submitted C# Code Syntax
     └───────────────────────────────┘     Returns Boolean Validation State
                     │
                     ▼
     ┌───────────────────────────────┐
     │  3. DISPATCHER TIMER ENGINE   │ ──► Asynchronous Exam & Assignment Clocks
     └───────────────────────────────┘     Real-Time Resource Decay
                     │
                     ▼
     ┌───────────────────────────────┐
     │  4. EF CORE ORM STATE LAYER   │ ──► Syncs Player, Portfolio, & Course Data
     └───────────────────────────────┘     to SQL Server (LocalDB)

```

## Key Technical Innovations

* **Custom Lexical Analyzer & Parser**: Engineered a standalone compiler module (`CodeToSurvive.DLL`) that tokenizes and parses player-submitted C# code during in-game assignments. This prevents the need for external API execution, validating syntax logic entirely offline through Abstract Syntax Tree (AST) evaluation.
* **Event-Driven UI & Collision Engine**: Utilizes WPF's rendering pipeline to manage a continuous game loop. Player movement is intercepted via low-level `KeyDown` events, dynamically updating Canvas coordinate matrices. The system computes 2D spatial overlaps using `Rect.IntersectsWith` against bounding boxes to trigger localized modal interactions instantly.
* **Relational State Persistence via EF Core**: Architected a robust Code-First database schema using Entity Framework Core 8.0. The system maps complex relational models—such as One-to-Many configurations between Players and Course Progress, and One-to-One bindings between Lessons and Assignments—ensuring ACID-compliant game saves and state recovery.

## 3. Technology Stack & Ecosystem

| Layer | Technologies Used | Purpose / Implementation |
| --- | --- | --- |
| Frontend / UI | C# 12, .NET 8.0, WPF (XAML) | Drives the graphical interface, hardware input event routing, and data-bound HUD updates. |
| Core Game Logic | Custom C# Tokenizer/Parser | Compiles and evaluates player code submissions natively without requiring external sandboxes. |
| Data Architecture | Entity Framework Core, SQL Server | Facilitates seamless Object-Relational Mapping (ORM) and persistent tracking of the player's economic and educational state. |

## 4. Deep-Dive Implementation Analysis

### Custom Code Parsing & Asynchronous Timers

Within the University and Software House modules, the application leverages `DispatcherTimer` instances hooked directly into the UI thread to manage assignment countdowns. Upon submission, the raw string is piped into the custom parser engine to validate structural logic.

```csharp
// Extracted from AssignmentWindow.xaml.cs showcasing the parsing pipeline
private void OpenEditor_Click(object sender, RoutedEventArgs e)
{
    CodeEditorWindow editor = new CodeEditorWindow();
    editor.ShowDialog();

    if (string.IsNullOrWhiteSpace(editor.SourceCode)) return;

    try
    {
        // 1. Lexical Analysis
        Tokenizer t = new Tokenizer(editor.SourceCode);
        
        // 2. Syntax Parsing
        Parser p = new Parser(t.Tokenize());
        var errors = p.Parse();

        // 3. Game State Mutation
        if (errors.Any())
        {
            MessageBox.Show("Your code contains errors. Fix and try again.");
            return;
        }
    }
    catch
    {
        MessageBox.Show("Compiler error — invalid submission.");
        return;
    }

    // Success State Processing
    _timer.Stop();
    IsCompleted = true;
    DialogResult = true;
}

```

### Algorithmic Profile & Complexity Map

* **Collision Detection Engine**: Iterates through an array of defined boundary rectangles. Because the number of interactable buildings is fixed, the spatial check operates in strict O(B) time (where B is the number of buildings), ensuring zero frame drops during rendering.
* **Lexical Tokenization**: Scans the player's source code string character-by-character, resulting in a linear O(N) complexity profile where N represents the total length of the submitted code string.
* **Database Query Resolution**: Leveraging Entity Framework Core's delayed execution and index-mapped primary keys (e.g., `PlayerId`, `CourseId`), state retrieval operations execute in O(log N) optimal lookup times.

**[LOGIC / FEATURE GIF INSTRUCTION]**: Record a clean 10-second screen recording showing the core gameplay loop: walk the character to the University, accept an assignment, type a quick snippet of valid code into the Code Editor, submit it, and capture the success notification alongside the energy drain/skill increase on the HUD. Convert to an optimized GIF and replace the placeholder below.

## 5. Deployment & Quickstart Guide

### Prerequisites

* Operating System: Windows 10 / 11 (required for WPF native rendering).
* Framework: .NET 8.0 SDK.
* Database: SQL Server LocalDB (installed with Visual Studio by default).

### Installation Steps

```bash
# 1. Clone the simulator repository
git clone [https://github.com/username/code-to-survive-simulator.git](https://github.com/username/code-to-survive-simulator.git)
cd code-to-survive-simulator

# 2. Apply EF Core Migrations to build the local database schema
dotnet ef database update

# 3. Build and launch the game client
dotnet run --project CodeToSurvive

```

```

```
