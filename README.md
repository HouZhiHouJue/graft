Graft
=====

A RAFT Election implementation in Go. More information on RAFT can be found in this [research paper](https://ramcloud.stanford.edu/wiki/download/attachments/11370504/raft.pdf
) and this [video](http://www.youtube.com/watch?v=YbZ3zDzDnrw&list=WL20FE97C942825E1E).

[![License MIT](https://img.shields.io/npm/l/express.svg)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/nats-io/graft.svg?branch=master)](http://travis-ci.org/nats-io/graft)
[![Coverage Status](https://coveralls.io/repos/github/nats-io/graft/badge.svg)](https://coveralls.io/github/nats-io/graft)

Overview
=====

RAFT is a consensus based algorithm that produces consistent state through replicated logs and leader elections.

Example usage of the election algorithm is to produce guaranteed leaders for N-wise scalability and elimination
of SPOF (Single Point of Failure) within a system.

## Example Usage

```go

ci := graft.ClusterInfo{Name: "health_manager", Size: 3}
rpc, err := graft.NewNatsRpc(&nats.DefaultOptions)
errChan := make(chan error)
stateChangeChan := make(chan StateChange)
handler := graft.NewChanHandler(stateChangeChan, errChan)

node, err := graft.New(ci, handler, rpc, "/tmp/graft.log");

// ...

if node.State() == graft.LEADER {
  // Process as a LEADER
}

select {
  case sc := <- stateChangeChan:
    // Process a state change
  case err := <- errChan:
    // Process an error, log etc.
}

```


## License

(The MIT License)

Copyright (c) 2013-2016 Apcera Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.
