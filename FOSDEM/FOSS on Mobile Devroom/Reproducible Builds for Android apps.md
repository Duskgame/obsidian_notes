Andreas Itzchak Rehberg (Izzy)

What are RB and what are they good for

- third parties make sure idependently that software hasnt been alered 
- make sure software always works the same way 
	makes software more consistenr and trustworhthy
- detect unauthorized changed to the build process
- ensure software complies with licenses and industry standards by proving binaries match their source code
- confirm ownership in case of signing key loss


How are RB approached at Izzyondroid

- RB on seperate Track
- failed RB does not prevent updates from being published
- using rbtlog
- using only FOSS tooling
- podman containers, different images
- multiple builders including independents
- independently verifying developers builds
- always shipping the APKs built and signes


What are the challenges

- Easy to handle java and kotin. in most cases only jdk version must match
- more complex apps with native libraries
	- build path embedded into native libs
	- path from windows builds cannot be matched on linux (drive letters)
	- different build  SDKs produce different output


What are the most frequent sources of failed RBs

- dirty builds different commit
- switching between building on windows and linux (line endings)
- introducing enforced signing 
- building locally with different setups  than specified
- embedded build timestamps
- using non-LTS JDKs 


What should Android App developers be aware of take care for

- always make release builds


reproducible-builds.org
