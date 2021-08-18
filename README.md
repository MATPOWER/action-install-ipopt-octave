Install IPOPT (Octave)
======================

### GitHub Action to build and install the [IPOPT][1] interface for
[GNU Octave][2]

Builds and installs the [Octave][2] interface for COIN-OR's [IPOPT][1] and
defines the `IPOPT_PATH` environment variable to point to the directory
containing the [IPOPT][1] MEX- and M-files.

__Note:__ This action depends on [Octave][2] being installed first, using either
[`MATPOWER/action-install-octave-linux@v1`][3] or
[`MATPOWER/action-install-octave-macos@v1`][4].

Tested on Linux and macOS runners.

### Inputs / Outputs

None.

### Example Usage

Linux:
```
    steps:
    - name: Install Octave (Linux)
      uses: MATPOWER/action-install-octave-linux@v1
      with:
        ipopt-libs: true

    - name: Octave ${{ env.ML_VER }} Installed
      run: $ML_CMD ver

    - name: Install IPOPT interface for Octave
      uses: MATPOWER/action-install-ipopt-octave@v1

    - name: Run IPOPT code in Octave
      run:  |
        export IPOPT_TEST_PATH=<directory-with-code-that-calls-IPOPT>
        env $ML_PATHVAR=$IPOPT_TEST_PATH $ML_CMD "<code-that-calls-IPOPT>"
        ls -al $IPOPT_PATH
```

macOS:
```
    steps:
    - name: Install Octave (macOS)
      uses: MATPOWER/action-install-octave-macos@v1

    - name: Octave ${{ env.ML_VER }} Installed
      run: $ML_CMD ver
    
    - name: Cache IPOPT Libs (macOS)
      id: cache-ipopt-libs
      uses: actions/cache@v2
      with:
        path: ~/install
        key: ${{ matrix.os }}-ipopt-libs

    - name: Build IPOPT (macOS)
      if: steps.cache-ipopt-libs.outputs.cache-hit != 'true'
      uses: MATPOWER/action-build-ipopt-macos@v1

    - name: Install IPOPT interface for Octave
      uses: MATPOWER/action-install-ipopt-octave@v1

    - name: Run IPOPT code in Octave
      run:  |
        export IPOPT_TEST_PATH=<directory-with-code-that-calls-IPOPT>
        env $ML_PATHVAR=$IPOPT_TEST_PATH $ML_CMD "<code-that-calls-IPOPT>"
        ls -al $IPOPT_PATH
```

[1]: https://github.com/coin-or/Ipopt
[2]: https://octave.org
[3]: https://github.com/MATPOWER/action-install-octave-linux
[4]: https://github.com/MATPOWER/action-install-octave-macos
