name: Run tests and report results
runs:
  using: composite
  steps:
    - name: Test
      run: ci/run_tests.sh
      shell: bash -el {0}

    - name: Publish test results
      uses: actions/upload-artifact@v4
      with:
        name: Test results
        path: test-data.xml
      if: failure()

    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v4
      with:
        flags: unittests
        name: codecov-pandas
        fail_ci_if_error: false
      if: failure()
