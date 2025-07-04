# Testament

Run test cases listed as JSON objects using`bash`or a shell of your liking.

---

### Arguments

<dl>

  <dt><h5>-l,--list</h5></dt>
  <dd>
	List all test cases in test suite.
  </dd>
  <dt><h5>-k,--keep-going</h5></dt>
  <dd>
    Continue with remaining test cases upon failure.
  </dd>
  <dt><h5>[[test_case_number ]]</h5></dt>
  <dd>
    Run test cases specified by number(s) separated by space(s).
  </dd>
  <dt><h5>[pattern]</h5></dt>
  <dd>
    Run test cases by matching entire name strings with specified pattern.
  </dd>

</dl>

---

### Test suite

A test suite consists of a file named `testsuite.json` containing JSON encoded information regarding a single or multiple test cases (JSON array of objects).
Test suite file is searched recursively starting from the directory containing the `testament` executable.

---

#### Variables

Commands to run test programs are provided in the test suite. These commands are executed in `bash` (shell used to run testament). Internal variables which are made available for the commands can referenced with `<variable-name>`.

<dl>

  <dt><h5>prjroot</h5></dt>
  <dd>
    Root directory for the project in case of a git repository. Otherwise
    set as the current working directory from where the test runner was
    launched.
  </dd>

  <dt><h5>PORT</h5></dt>
  <dd>
    The port specified to run a service on. (see "port" parameter)
  </dd>

</dl>

---

#### Test case parameters

A sample test suite file containing a single test case to print a random number on the terminal.

```json
[
  {
    "name": "Print random number",
    "sandbox": false,
    "precmd": "",
    "port": "",
    "proto": "",
    "cap_sent_port": "",
    "cap_recv_port": "",
    "cap_sent_filter": "",
    "cap_recv_filter": "",
    "progcmd": "",
    "testcmd": [ "echo random number: $(($RANDOM % 100))" ],
    "filter" : [ "" ],
    "matchfilter" : [ "" ],
    "prelaunchtest": false,
    "chkserver": false,
    "compile": false,
    "builddir": "",
    "progname_exact": "",
    "progname_pattern": "",
    "progkill_sigterm": false,
    "postcmd": "",
    "pass": [ [ "random number: [0-99]" ] ],
    "passregex": true,
    "fail": [ [ "" ] ],
    "failregex": false
  }
]
```

---

Each test case requires parameters which are provided as keys in the JSON object. The following keys **must** be present for each test case.

<dl>

  <dt><h5>name</h5></dt>
  <dd>
    String. Name of the test case. It should be unique and can be used to
    invoke individual test cases.
  </dd>

  <dt><h5>sandbox</h5></dt>
  <dd>
    Boolean. Specifies whether the compilation (if needed) and test run
    are performed in the /tmp folder.
  </dd>

  <dt><h5>precmd</h5></dt>
  <dd>
    String. Prerequisite command. It will be run before the test commands.
  </dd>

  <dt><h5>port</h5></dt>
  <dd>
    String. Specifies the port on which a service shall be started.
  </dd>

  <dt><h5>proto</h5></dt>
  <dd>
    String. Specifies the protocol to apply filters on data transfered via the
    service. Admissible values are "HTTP", "http", "TCP", "tcp", "UDP", "udp".
  </dd>

  <dt><h5>cap_sent_port</h5></dt>
  <dd>
    String. Specified the port on which sent TCP traffic shall be
    captured.
  </dd>

  <dt><h5>cap_recv_port</h5></dt>
  <dd>
    String. Specified the port on which received TCP traffic shall be
    captured.
  </dd>

  <dt><h5>cap_sent_filter</h5></dt>
  <dd>
    String. Specified the filter to be used for sent TCP traffic.
    For filters, see section below.
  </dd>

  <dt><h5>cap_recv_filter</h5></dt>
  <dd>
    String. Specified the filter to be used for received TCP traffic.
    For filters, see section below.
  </dd>

  <dt><h5>progcmd</h5></dt>
  <dd>
    String. The command to run the program to be tested.
  </dd>

  <dt><h5>testcmd</h5></dt>
  <dd>
    Array of strings. Commands to be run for the test case.
  </dd>

  <dt><h5>filter</h5></dt>
  <dd>
    String. Specifies the filter applied to output of test commands for
    printing.
  </dd>

  <dt><h5>matchfilter</h5></dt>
  <dd>
    String. Specifies the filter applied to output of test commands to
    match with expected results.
  </dd>

  <dt><h5>prelaunchtest</h5></dt>
  <dd>
    Boolean. Specifies whether test commands need to launched before
    starting the test. This can be useful in cases where the test
    command(s) is(are) waiting for some event or input.
  </dd>

  <dt><h5>chkserver</h5></dt>
  <dd>
    Boolean. Specifies whether a test need to be performed for a service
    running on specified port (see "port" parameter).
  </dd>

  <dt><h5>compile</h5></dt>
  <dd>
    Boolean. Specifies whether compilation is needed for the project.
  </dd>

  <dt><h5>builddir</h5></dt>
  <dd>
    String. Specifies the build dir if compilation is needed.
  </dd>

  <dt><h5>progname_exact</h5></dt>
  <dd>
    String. Specifies the exact name of the program to be tested
    (see "progcmd" parameter). This is used to kill the program.
  </dd>

  <dt><h5>progname_pattern</h5></dt>
  <dd>
    String. Specifies a regex pattern to match the program to be tested
    (see "progcmd" parameter). This is used to kill the program.
  </dd>

  <dt><h5>progkill_sigterm</h5></dt>
  <dd>
    Boolean. Specifies whether SIGTERM will be used to kill the program,
    SIGKILL shall be used otherwise.
  </dd>

  <dt><h5>postcmd</h5></dt>
  <dd>
    String. It will be run after the test commands.
  </dd>

  <dt><h5>pass</h5></dt>
  <dd>
    Array of strings. Specifies the pattern(s) to be matched agains the
    filtered output of test command(s) for success.
  </dd>

  <dt><h5>passregex</h5></dt>
  <dd>
    Boolean. Specifies if pass pattern(s) is(are) regex patterns.
  </dd>

  <dt><h5>fail</h5></dt>
  <dd>
    Array of strings. Specifies the pattern(s) to be matched agains the
    filtered output of test command(s) for failure.
  </dd>

  <dt><h5>failregex</h5></dt>
  <dd>
    Boolean. Specifies if fail pattern(s) is(are) regex patterns.
  </dd>

</dl>

---

### Sample run

<pre style="font-size: 14px;">
$ export PATH="$PATH:$(pwd)"
$ testament.run
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test #1 : &quot;Print message&quot;</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">PORT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Running program</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">echo Hello, World!</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">testcmd:</span>
<span style="color:olive;">your program: </span><span style="color:blue;">Hello, World!</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">OUTPUT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;"></span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test passed</span>

<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test #2 : &quot;Print random number&quot;</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">PORT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">testcmd: echo random number: $(($RANDOM % 100))</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">OUTPUT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">random number: 49</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test passed</span>

<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test #3 : &quot;Send TCP data&quot;</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">PORT: 1234</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">testcmd: nc -lp 1234</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">postcmd: echo 1234:hello | nc -N localhost 1234</span>
<span style="color:olive;">tester: </span><span style="color:teal;">TCP: Port &quot;1234&quot;: Sent bytes: 1234:hello\x0a</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">OUTPUT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">1234:hello</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test passed</span>

<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test #4 : &quot;Send UDP data&quot;</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">PORT: 1234</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">testcmd: nc -W 1 -ulp 1234</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:blue;">postcmd: echo 1234:hello &gt; /dev/udp/localhost/1234</span>
<span style="color:olive;">tester: </span><span style="color:teal;">UDP: Port &quot;1234&quot;: Sent bytes: 1234:hello\x0a</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">OUTPUT:</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:green;">1234:hello</span>
<span style="color:olive;">tester: </span><span style="filter: contrast(70%) brightness(190%);color:teal;">Test passed</span>

All tests passed
</pre>
