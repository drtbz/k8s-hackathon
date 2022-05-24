# Dockerfile - Level II (multi stage builds using a simple dotnet core app)

> ### You'll be doing this when building the app as well, so feel free to search for dockerfile examples based on the programming language / framework of your choice. Below `dotnet` example is just for reference. 

## Containerize a simple dotnet app

1. Create a folder called `my-apps` and `cd` into it.

    ```bash
        mkdir my-apps && cd my-apps
    ```

2. Create a console app using dotnet cli and call it `strings-app`

    ```bash
        dotnet new console -n strings-app 
    ```

3. Create a `Dockerfile` in the `my-apps` directory and enter the below.

> **Recommended** - Instead of using the below, try generating a Dockerfile via Docker Extension for VS Code. This will also ensure you get the latest version of dotnet multi-stage template, which you can always tweak it after.

    ```Dockerfile

        FROM mcr.microsoft.com/dotnet/core/runtime:3.1-buster-slim AS base
        WORKDIR /app

        FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
        WORKDIR /src
        COPY ["strings-app/strings-app.csproj", "strings-app/"]
        RUN dotnet restore "strings-app/strings-app.csproj"
        COPY . .
        WORKDIR "/src/strings-app"
        RUN dotnet build "strings-app.csproj" -c Release -o /app/build

        FROM build AS publish
        RUN dotnet publish "strings-app.csproj" -c Release -o /app/publish

        FROM base AS final
        WORKDIR /app
        COPY --from=publish /app/publish .
        ENTRYPOINT ["dotnet", "strings-app.dll"]

    ```

2. Open `Program.cs` inside `strings-app` directory on vs code editor and Replace the `Main` methos, so it looks like below

```csharp
        static void Main(string[] args)
        {
            while(true) {            
                Console.WriteLine("Enter a string to check it's length or type quit to exit the app:");
                var input = Console.ReadLine();
                if (input.ToLower() == "quit") { break; }
                Console.WriteLine(input.Length);
            }
        }
```

3. Ensure the folder structure looks like below. Notice that the `Dockerfile` is one level up.

```bash
.
└── my-apps
    ├── Dockerfile
    └── strings-app
        ├── Program.cs
        ├── obj             
        └── strings-app.csproj
```

4. Build your Docker Image from `my-apps` folder. 

```bash
    docker build . -t strings-app:1.0    
```

5. Now run your app and interact with it.

```bash
    docker run -it --rm strings-app:1.0
    # --rm will automatically remove the container when it exits
```
#### Dockerfile Authoring Best Practices
 * https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---

### Optional: Clean up

>Important: As with any system, use clean up commands with caution. 

> **Do not run** below commands if you have other images and containers in your workstation that you do not wish to delete! Instead, just remove the images and containers for above `strings-app`

> Do not force delete images that are still referenced by containers. 

#### To clean up dangling resources
```bash
docker system prune
```

#### To clean up **all** resources including images,containers,volumes, etc. 
```
docker system prune --all

# `y' when prompted
```