# Neo4j Installation

**Download:** Neo4j 2026, Windows zip — <https://neo4j.com/deployment-center/> (Community)

This walk-through is using `neo4j-community-2026.09.0`.

**System Requirements and OracleJDK**
- <https://neo4j.com/docs/operations-manual/current/installation/requirements/>
- <https://www.oracle.com/java/technologies/downloads/#jdk21-windows>

**Install (instructions used are below):** <https://neo4j.com/docs/operations-manual/current/installation/windows/>

**AuraDB (not used):** <https://neo4j.com/product/auradb/>

---

## Neo4j Operations Manual — Windows Installation

Before you install Neo4j on Windows, check System Requirements to see if your
setup is suitable. If it is not already installed, get OracleJDK 21 or ZuluJDK 21.
Java 25 is also supported.

Go with the x64 Installer.

### Install and start Neo4j

You can install Neo4j on Windows either by downloading and extracting a ZIP
archive, by running it as a Windows service, or by using the Windows PowerShell
module.

### Install Neo4j using a zip archive

1. Download the **Windows Executable 2026.09.0 (zip)** release from the Neo4j
   Deployment Center. (`neo4j-community-2026.09.0`)
2. Right-click the downloaded file and select **Extract All** to extract the
   contents of the archive.
3. Drill into this folder until you find the folder called
   `neo4j-community-2026.09.0`. For example: after extracting the zip file, you
   end up with a folder called `neo4j-community-2026.09.0-windows`. Open this
   folder and find the folder you actually need: `neo4j-community-2026.09.0`.
4. Place the extracted folder (`neo4j-community-2026.09.0`) in a permanent home
   on your local machine (strongly recommend the root of `C:` for Windows
   users) and set the environment variable `NEO4J_HOME` to point to the
   extracted directory, for example (command prompt):

   ```
   setx NEO4J_HOME "C:\neo4j-community-2026.09.0"
   ```

   > **Note:** the following image references a previous version. Please be
   > aware of the version you are using and adjust the instructions
   > appropriately.

5. Close, reopen, or restart your command prompt so `%NEO4J_HOME%` is
   initiated.
6. Start Neo4j as a console application by running
   `%NEO4J_HOME%\bin\neo4j console` in your command prompt:

   ```
   C:\neo4j-community-2026.09.0\bin\neo4j console
   ```

7. Then go to: <http://localhost:7474/browser/>
8. Default user: `neo4j`; password: `neo4j`
