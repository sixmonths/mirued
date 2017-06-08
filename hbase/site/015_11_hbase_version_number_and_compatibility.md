11. HBase version number and compatibility
HBase has two versioning schemes, pre-1.0 and post-1.0. Both are detailed below.

11.1. Post 1.0 versions
Starting with the 1.0.0 release, HBase is working towards Semantic Versioning for its release versioning. In summary:

Given a version number MAJOR.MINOR.PATCH, increment the:
MAJOR version when you make incompatible API changes,

MINOR version when you add functionality in a backwards-compatible manner, and

PATCH version when you make backwards-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

Compatibility Dimensions
In addition to the usual API versioning considerations HBase has other compatibility dimensions that we need to consider.

Client-Server wire protocol compatibility
Allows updating client and server out of sync.

We could only allow upgrading the server first. I.e. the server would be backward compatible to an old client, that way new APIs are OK.

Example: A user should be able to use an old client to connect to an upgraded cluster.

Server-Server protocol compatibility
Servers of different versions can co-exist in the same cluster.

The wire protocol between servers is compatible.

Workers for distributed tasks, such as replication and log splitting, can co-exist in the same cluster.

Dependent protocols (such as using ZK for coordination) will also not be changed.

Example: A user can perform a rolling upgrade.

File format compatibility
Support file formats backward and forward compatible

Example: File, ZK encoding, directory layout is upgraded automatically as part of an HBase upgrade. User can rollback to the older version and everything will continue to work.

Client API compatibility
Allow changing or removing existing client APIs.

An API needs to be deprecated for a major version before we will change/remove it.

APIs available in a patch version will be available in all later patch versions. However, new APIs may be added which will not be available in earlier patch versions.

New APIs introduced in a patch version will only be added in a source compatible way [1]: i.e. code that implements public APIs will continue to compile.

Example: A user using a newly deprecated API does not need to modify application code with HBase API calls until the next major version. *

Client Binary compatibility
Client code written to APIs available in a given patch release can run unchanged (no recompilation needed) against the new jars of later patch versions.

Client code written to APIs available in a given patch release might not run against the old jars from an earlier patch version.

Example: Old compiled client code will work unchanged with the new jars.

If a Client implements an HBase Interface, a recompile MAY be required upgrading to a newer minor version (See release notes for warning about incompatible changes). All effort will be made to provide a default implementation so this case should not arise.

Server-Side Limited API compatibility (taken from Hadoop)
Internal APIs are marked as Stable, Evolving, or Unstable

This implies binary compatibility for coprocessors and plugins (pluggable classes, including replication) as long as these are only using marked interfaces/classes.

Example: Old compiled Coprocessor, Filter, or Plugin code will work unchanged with the new jars.

Dependency Compatibility
An upgrade of HBase will not require an incompatible upgrade of a dependent project, including the Java runtime.

Example: An upgrade of Hadoop will not invalidate any of the compatibilities guarantees we made.

Operational Compatibility
Metric changes

Behavioral changes of services

JMX APIs exposed via the /jmx/ endpoint

Summary
A patch upgrade is a drop-in replacement. Any change that is not Java binary and source compatible would not be allowed.[2] Downgrading versions within patch releases may not be compatible.

A minor upgrade requires no application/client code modification. Ideally it would be a drop-in replacement but client code, coprocessors, filters, etc might have to be recompiled if new jars are used.

A major upgrade allows the HBase community to make breaking changes.

Table 3. Compatibility Matrix [3]
Major
Minor
Patch
Client-Server wire Compatibility
N
Y
Y
Server-Server Compatibility
N
Y
Y
File Format Compatibility
N [4]
Y
Y
Client API Compatibility
N
Y
Y
Client Binary Compatibility
N
N
Y
Server-Side Limited API Compatibility
Stable
N
Y
Y
Evolving
N
N
Y
Unstable
N
N
N
Dependency Compatibility
N
Y
Y
Operational Compatibility
N
N
Y
11.1.1. HBase API Surface
HBase has a lot of API points, but for the compatibility matrix above, we differentiate between Client API, Limited Private API, and Private API. HBase uses a version of Hadoop’s Interface classification. HBase’s Interface classification classes can be found here.

InterfaceAudience: captures the intended audience, possible values are Public (for end users and external projects), LimitedPrivate (for other Projects, Coprocessors or other plugin points), and Private (for internal use).

InterfaceStability: describes what types of interface changes are permitted. Possible values are Stable, Evolving, Unstable, and Deprecated. Notice that this annotation is only valid for classes which are marked as IA.LimitedPrivate. The stability of IA.Public classes is only related to the upgrade type(major, minor or patch). And for IA.Private classes, there is no guarantee on the stability between releases. Refer to the Compatibility Matrix above for more details.

HBase Client API
HBase Client API consists of all the classes or methods that are marked with InterfaceAudience.Public interface. All main classes in hbase-client and dependent modules have either InterfaceAudience.Public, InterfaceAudience.LimitedPrivate, or InterfaceAudience.Private marker. Not all classes in other modules (hbase-server, etc) have the marker. If a class is not annotated with one of these, it is assumed to be a InterfaceAudience.Private class.
HBase LimitedPrivate API
LimitedPrivate annotation comes with a set of target consumers for the interfaces. Those consumers are coprocessors, phoenix, replication endpoint implementations or similar. At this point, HBase only guarantees source and binary compatibility for these interfaces between patch versions.
HBase Private API
All classes annotated with InterfaceAudience.Private or all classes that do not have the annotation are for HBase internal use only. The interfaces and method signatures can change at any point in time. If you are relying on a particular interface that is marked Private, you should open a jira to propose changing the interface to be Public or LimitedPrivate, or an interface exposed for this purpose.
11.2. Pre 1.0 versions
HBase Pre-1.0 versions are all EOM
For new installations, do not deploy 0.94.y, 0.96.y, or 0.98.y. Deploy our stable version. See EOL 0.96, clean up of EOM releases, and the header of our downloads.
Before the semantic versioning scheme pre-1.0, HBase tracked either Hadoop’s versions (0.2x) or 0.9x versions. If you are into the arcane, checkout our old wiki page on HBase Versioning which tries to connect the HBase version dots. Below sections cover ONLY the releases before 1.0.

Odd/Even Versioning or "Development" Series Releases
Ahead of big releases, we have been putting up preview versions to start the feedback cycle turning-over earlier. These "Development" Series releases, always odd-numbered, come with no guarantees, not even regards being able to upgrade between two sequential releases (we reserve the right to break compatibility across "Development" Series releases). Needless to say, these releases are not for production deploys. They are a preview of what is coming in the hope that interested parties will take the release for a test drive and flag us early if we there are issues we’ve missed ahead of our rolling a production-worthy release.

Our first "Development" Series was the 0.89 set that came out ahead of HBase 0.90.0. HBase 0.95 is another "Development" Series that portends HBase 0.96.0. 0.99.x is the last series in "developer preview" mode before 1.0. Afterwards, we will be using semantic versioning naming scheme (see above).

Binary Compatibility
When we say two HBase versions are compatible, we mean that the versions are wire and binary compatible. Compatible HBase versions means that clients can talk to compatible but differently versioned servers. It means too that you can just swap out the jars of one version and replace them with the jars of another, compatible version and all will just work. Unless otherwise specified, HBase point versions are (mostly) binary compatible. You can safely do rolling upgrades between binary compatible versions; i.e. across point versions: e.g. from 0.94.5 to 0.94.6. See link:[Does compatibility between versions also mean binary compatibility?] discussion on the HBase dev mailing list.

11.3. Rolling Upgrades
A rolling upgrade is the process by which you update the servers in your cluster a server at a time. You can rolling upgrade across HBase versions if they are binary or wire compatible. See Rolling Upgrade Between Versions that are Binary/Wire Compatible for more on what this means. Coarsely, a rolling upgrade is a graceful stop each server, update the software, and then restart. You do this for each server in the cluster. Usually you upgrade the Master first and then the RegionServers. See Rolling Restart for tools that can help use the rolling upgrade process.

For example, in the below, HBase was symlinked to the actual HBase install. On upgrade, before running a rolling restart over the cluster, we changed the symlink to point at the new HBase software version and then ran

$ HADOOP_HOME=~/hadoop-2.6.0-CRC-SNAPSHOT ~/hbase/bin/rolling-restart.sh --config ~/conf_hbase
The rolling-restart script will first gracefully stop and restart the master, and then each of the RegionServers in turn. Because the symlink was changed, on restart the server will come up using the new HBase version. Check logs for errors as the rolling upgrade proceeds.

Rolling Upgrade Between Versions that are Binary/Wire Compatible
Unless otherwise specified, HBase point versions are binary compatible. You can do a Rolling Upgrades between HBase point versions. For example, you can go to 0.94.6 from 0.94.5 by doing a rolling upgrade across the cluster replacing the 0.94.5 binary with a 0.94.6 binary.

In the minor version-particular sections below, we call out where the versions are wire/protocol compatible and in this case, it is also possible to do a Rolling Upgrades. For example, in Rolling upgrade from 0.98.x to HBase 1.0.0, we state that it is possible to do a rolling upgrade between hbase-0.98.x and hbase-1.0.0.