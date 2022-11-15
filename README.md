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

### Optional Input

- `cached` - (default `false`) set to `true` to indicate that you are
  installing from a cached build, i.e. skip the building and just define
  `IPOPT_PATH` and install it in the Octave path.

### Outputs

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

    - name: Cache IPOPT interface for Octave
      id: cache-ipopt
      env:
        cache-name: ipopt
      uses: actions/cache@v3
      with:
        path: ~/build/ipopt
        key: ${{ matrix.os }}-${{ env.cache-name }}

    - name: Install IPOPT interface for Octave
      uses: MATPOWER/action-install-ipopt-octave@v1
      with:
        cached: ${{ steps.cache-ipopt.outputs.cache-hit == 'true' }}

    - name: Run IPOPT code in Octave
      run: |
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
      env:
        cache-name: ipopt-libs
      uses: actions/cache@v3
      with:
        path: ~/install
        key: ${{ matrix.os }}-${{ env.cache-name }}

    - name: Build IPOPT (macOS)
      if: steps.cache-ipopt-libs.outputs.cache-hit != 'true'
      uses: MATPOWER/action-build-ipopt-macos@v1

    - name: Cache IPOPT interface for Octave
      id: cache-ipopt
      env:
        cache-name: ipopt
      uses: actions/cache@v3
      with:
        path: ~/build/ipopt
        key: ${{ matrix.os }}-${{ env.cache-name }}

    - name: Install IPOPT interface for Octave
      uses: MATPOWER/action-install-ipopt-octave@v1
      with:
        cached: ${{ steps.cache-ipopt.outputs.cache-hit == 'true' }}

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
