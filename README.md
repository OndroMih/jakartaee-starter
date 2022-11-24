Welcome to the official Eclipse Foundation starter for Jakarta EE. The starter is a Maven Archetype that generates
sample code to get you going quickly with simple Jakarta EE microservices projects. The starter will include a web UI in
a subsequent release.

## Generate Jakarta EE Project

In order to run the Maven Archetype and generate a sample Jakarta EE project, please execute the following. Please
ensure you have installed a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8)
and [Maven 3+](https://maven.apache.org/download.cgi) (we have tested with Java SE 8, Java SE 11 and Java SE 17).


## Generate a Jakarta EE Project

<script>
function getFormData() {
    const mavenArchetype = document.getElementById("mavenArchetype").value;
    const mvnArchetypeArray = mavenArchetype.split(",");
    return {
        mavenArchetype: mavenArchetype,
        mvnArchetypeGroupId: mvnArchetypeArray[0],
        mvnArchetypeArtifactId: mvnArchetypeArray[1],
        mvnArchetypeVersion:  mvnArchetypeArray[2],
        profile: document.getElementById("profile").value,
        groupId: document.getElementById("groupId").value,
        artifactId: document.getElementById("artifactId").value,
        projectVersion: document.getElementById("projectVersion").value,
    };
}
function generateMvnCommand() {
    const formData = getFormData();

    const mvnArchetypeGenerate = document.getElementById("mvnArchetypeGenerate");

    if (!formData.mavenArchetype || !formData.groupId || !formData.artifactId || !formData.projectVersion) {
        mvnArchetypeGenerate.value = "Please fill in all fields";
        return;
    }

    mvnArchetypeGenerate.value = `mvn archetype:generate -DarchetypeGroupId=${formData.mvnArchetypeGroupId} -DarchetypeArtifactId=${formData.mvnArchetypeArtifactId} -DarchetypeVersion=${formData.mvnArchetypeVersion} -DgroupId=${formData.groupId} -DartifactId=${formData.artifactId} -Dprofile=${formData.profile} -Dversion=${formData.projectVersion} -DinteractiveMode=false`;
}

function copyMvnCommand() {
    const mvnArchetypeGenerate = document.getElementById("mvnArchetypeGenerate");
    mvnArchetypeGenerate.select();
    mvnArchetypeGenerate.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(document.getElementById("mvnArchetypeGenerate").value);
}

function download() {
    const path = "http://localhost:8080/starter-ui/download.zip";
    const filename = "jakartaee-cafe.zip";
    const formData = getFormData();

    // Create a new link
    const anchor = document.createElement('a');
    anchor.href = path + `?archetypeGroupId=${formData.mvnArchetypeGroupId}&archetypeArtifactId=${formData.mvnArchetypeArtifactId}&archetypeVersion=${formData.mvnArchetypeVersion}&groupId=${formData.groupId}&artifactId=${formData.artifactId}&profile=${formData.profile}&version=${formData.projectVersion}`;
    anchor.download = filename;

    // Append to the DOM
    document.body.appendChild(anchor);

    // Trigger `click` event
    anchor.click();

    // Remove element from DOM
    document.body.removeChild(anchor);
}; 

</script>

<form onchange="generateMvnCommand()">
    <div class="form-row">
        <div class="form-group" >
            <label for="mavenArchetype">Jakarta EE version</label>
            <select class="form-control" id="mavenArchetype" onchange="generateMvnCommand()">
                <option value="org.eclipse.starter,jakartaee10-minimal,1.1.0">Jakarta EE 10</option>
                <option value="org.eclipse.starter,jakartaee9.1-minimal,1.0.0">Jakarta EE 9.1</option>
                <option value="org.eclipse.starter,jakartaee8-minimal,1.0.0">Jakarta EE 8</option>
            </select>
        </div>
        <div class="form-group" >
            <label for="profile">Profile</label>
            <select class="form-control" id="profile" onchange="generateMvnCommand()">
                <option value="api">Platform</option>
                <option value="web-api">Web Profile</option>
                <option value="core-api">Core Profile</option>
            </select>
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label for="groupId">Group</label>
            <input class="form-control" type="text" id="groupId" placeholder="com.example" onchange="generateMvnCommand()">
        </div>
        <div class="form-group">
            <label for="artifactId">Artifact</label>
            <input type="text" class="form-control" id="artifactId" placeholder="demo" onchange="generateMvnCommand()">
        </div>
        <div class="form-group">
            <label for="projectVersion">Version</label>
            <input type="text" class="form-control" id="projectVersion" value="1.0.0-SNAPSHOT" onchange="generateMvnCommand()">
        </div>
    </div>
    <div class="form-row">
        <button onclick="download();">Generate ZIP</button>
    </div>
    <div class="form-group">
        <label for="mvnArchetypeGenerate">
            Generate the project using Maven on command line
        </label>
        <p>Just copy this command as is into a terminal where you want to start your project and press Enter...</p>
        <textarea class="form-control"
                  id="mvnArchetypeGenerate"
                  rows="11"
                  readonly
                  aria-describedby="mvnCommandHelp"
                  onclick="copyMvnCommand()">
        </textarea>
        <small id="mvnCommandHelp" class="form-text text-muted">By clicking on the text it will be copied to the
            clipboard
        </small>
    </div>
</form>

<script>
    generateMvnCommand();
</script>


## Example projects

If desired, you can easily use the Maven Archetype from a Maven capable IDE such
as [Eclipse](https://www.eclipse.org/ide).

If you use the defaults, this will generate the Jakarta EE project under a directory named `jakartaee-cafe`. You can
then run the project by executing the following command from the `jakartaee-cafe` directory. Please ensure you have
installed a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8)
and [Maven 3+](https://maven.apache.org/download.cgi) (we have tested with Java SE 8, Java SE 11 and Java SE 17).

```
mvn clean package payara-micro:start
```

Once Payara Micro starts, you can access the project at http://localhost:8080.

You can also run the project via Docker. To build the Docker image, execute the following commands from
the `jakartaee-cafe` directory. Please ensure you have installed
a [Java SE 8+ implementation](https://adoptium.net/?variant=openjdk8), [Maven 3+](https://maven.apache.org/download.cgi)
and [Docker](https://docs.docker.com/get-docker/) (we have tested with Java SE 8, Java SE 11 and Java SE 17).

```
mvn clean package
docker build -t jakartaee-cafe:v1 .
```

You can then run the Docker image by executing:

```
docker run -it --rm -p 8080:8080 jakartaee-cafe:v1
```

Once Payara starts, you can access the project at http://localhost:8080/jakartaee-cafe.

The generated starter code is simply a Maven project. You can easily load, explore and run the code in a Maven capable
IDE such as [Eclipse](https://www.eclipse.org/ide).

We hope you enjoy your Jakarta EE journey!

## Learn more!

There are many excellent free resources to learn Jakarta EE! The following are some that you should begin exploring
alongside the starter.

* The [Jakarta EE Tutorial](https://eclipse-ee4j.github.io/jakartaee-tutorial) is a comprehensive reference for
  developing applications with Jakarta EE.
* The [First Cup](https://eclipse-ee4j.github.io/jakartaee-firstcup/) is part of the Tutorial and is a gentle hands-on
  introduction to Jakarta EE.
* You can further explore the [First Cup Examples](https://github.com/eclipse-ee4j/jakartaee-firstcup-examples) to get a
  feel for how Jakarta EE applications look like.
* The [Jakarta EE Tutorial Examples](https://github.com/eclipse-ee4j/jakartaee-tutorial-examples) is a very
  comprehensive resource showing you how to use many Jakarta EE APIs and features.
* The [Cargo Tracker](https://eclipse-ee4j.github.io/cargotracker/) project demonstrates first-hand how you can develop
  applications with Jakarta EE using widely adopted architectural best practices like Domain-Driven Design (DDD).

