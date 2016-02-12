# ![alt text](https://avatars2.githubusercontent.com/u/1263946?v=3&s=84 "KBase") KBase SDK - Locally Test Module and Method(s)


### <A NAME="test-module-and-methods"></A>5. Locally Test Module and Method(s)


#### 5A. Edit Dockerfile

The base KBase Docker image contains a KBase Ubuntu image, but not much else.  You will need to add whatever dependencies, including the installation of whatever tool you are wrapping, to the Dockerfile that is executed to build a custom Docker image that can run your Module.

For example:

    RUN git clone https://github.com/torognes/vsearch
    WORKDIR vsearch
    RUN ./configure 
    RUN make
    RUN make install
    WORKDIR ../

You will also need to add your KBase SDK module to the Dockerfile.  For example:

    RUN mkdir -p /kb/module/test
    WORKDIR test
    RUN git clone https://github.com/dcchivian/kb_vsearch_test_data
    WORKDIR ../

#### 5B. Build tests of your methods

Edit the local test config file (`test_local/test.cfg`) with a KBase user account name and password (note that this directory is in .gitignore so will not be copied):

    test_user = TEST_USER_NAME
    test_password = TEST_PASSWORD

Run tests:

    cd test_local
    kb-sdk test

This will build your Docker container, run the method implementation running in the Docker container that fetches example ContigSet data from the KBase CI database and generates output.

Inspect the Docker container by dropping into a bash console and poke around, from the `test_local` directory:
    
    ./run_bash.sh
