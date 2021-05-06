# snippits for charp approvals workshop

## before starting the workshop

start docker containers
go to each container and run
```
cd project/[gitRepo]/[solutionFolder]
dotnet restore --disable-parallel
```

## Run tests with coverage

dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=lcov /p:CoverletOutput=./coverage/

## Cut & paste to save time

private static string DoUpdate(string name, int sellIn, int quality)
        {
            GildedRose gr = new GildedRose(new Item[] {
            new Item(name, sellIn, quality, "weapon", 1, 1,
                new Character("fred", "male", 1, "warrior", "elf"),
                new Realm("Atlantis"))
           });

            gr.UpdateQuality();
            var result = gr.Items[0].ToString();
            return result;
        }

# configure Stryker mutator tool

dotnet new tool-manifest
dotnet tool install dotnet-stryker
dotnet tool restore

## handy vs code keyboard shortcuts

Cmd+. - Quick Fix
Cmd + - Show parameter hints
F2 - Rename
Cmd + K Cmd + S - Keyboard shortcuts
Cmd + , - checkProjectSettingsExclusions
Cmd + 7 - show coverage
Cmd + 9 - Hide coverage
alt-Z - Toggle word wrap

To use web proxy

in terminal window
```
sudo service restart nginx
```

http://[ipAddress:port]/proxy/5000 (may have to manually re-enter the port 5000 string when navigating in the browser)