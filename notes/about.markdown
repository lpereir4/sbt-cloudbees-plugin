CloudBees Run@Cloud SBT Plugin
------------------------------

Integration for SBT that lets you deploy apps to the CloudBees RUN@Cloud PaaS

Usage
-----

Firstly, you need to grab the key and secret from grandcentral.cloudbees.com, which should look like:

![Grand Central Keys](https://github.com/timperrett/sbt-cloudbees-plugin/raw/master/notes/img/beehive-keys.jpg)

Once you have these two values, you can do one of two things:

* Enter them when the plugin prompts you; this will be on everytime you run a deployment to the cloud
* Create a properties file called bees.config in $HOME/.bees/ so that you only need to define them once per computer. This properties file needs to be a key-value pair style like this:

<pre><code>bees.api.key=XXXXXXXXXX
bees.api.secret=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
</code></pre>

Whichever route you choose to specify that information, you then only need to define the plugin information in any given project. Specifically, in the Plugins.scala file define the following:

<pre><code>import sbt._
  class Plugins(info: ProjectInfo) extends PluginDefinition(info) {
    lazy val cloudbees = "eu.getintheloop" % "sbt-cloudbees-plugin" % "0.2.7"
    lazy val sonatypeRepo = "sonatype.repo" at "https://oss.sonatype.org/content/groups/public"
  }
</code></pre>
 
Add the plugin to your SBT project like so:

<pre><code>import sbt._
  class YourProject(info: ProjectInfo) extends DefaultWebProject(info) with bees.RunCloudPlugin {
    ....
    override def beesApplicationId = Some("whatever")
    override def beesUsername = Some("youruser")
  }
</code></pre>

Again, if you would prefer to enter these values when you deploy your application then you can of course just enter the appropriate values when prompted. Now your all configured and good to go, there are two commands you can run with this plugin:

* Get a list of your configured applications: <code>bees-applist</code>
* Deploy your application <code>bees-deploy</code>

