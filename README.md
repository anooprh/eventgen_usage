## A Tutorial of how to use eventgen to generate random events on Splunk

#### [OPTIONAL] Step 1 : Setup a python virtual environment

Follow the [steps in gist](https://gist.github.com/anooprh/6abfa66fbd96f6065f3d86e1bcb14cbc) to create the virtualenv

#### Step 2 : Install Eventgen via pip

Follow the steps in gist to create the virtualenv

`$ pip install git+https://www.github.com/splunk/eventgen.git`

To verify that you have a `splunk_eventgen` installed, execute it at the command line.

```
splunk_eventgen --version
Eventgen 7.1.0
```

#### Step 3 : Prepare the eventgen.conf file and the sample file. 

For eventgen to work correctly, it needs atleast 2 files. First one is the `eventgen.conf` file ([spec](http://splunk.github.io/eventgen/REFERENCE.html#eventgenconfspec) and the second is a sample log line which we want to replicate)

Each stanza in `eventgen.conf` requires a header which should be a sample file name. We need to specify the `sampleDir` as well as we are using eventgen in standalone mode. 

The events in this example will follow the format 

```
2012-09-14 16:30:20,072 transType=ReplaceMe transID=000000 transGUID=0A0B0C userName=bob city="City" state=State zip=00000 value=0
```

where each of the fields are replaced. 

#### Step 4 : Perform a dry-run with outputmode=stdout

`splunk_eventgen` allows for several output modes (stdout, file, splunkstream, HEC, tcpout, udpout and so on --> Full List at http://splunk.github.io/eventgen/CONFIGURE.html#output-related-settings). 

In this `eventgen.conf`, the `outputMode` is set to `stdout` for the dry run. 

```
$ splunk_eventgen generate eventgen.conf
```

After this, we should start seeing the logs in stdout in the desired format. 

#### Step 5 : Set the correct outputMode to point to the splunk indexer

We can comment the `outputMode = stdout` and uncomment the subsequent lines which sets the `outputMode = splunkstream`. We need to specify the splunk indexer host, username and password for the events to be correctly received in the indexer. 

#### Step 6 : Verify that these events are being written to the splunk index. 

On the searchhead, we can search for `index = "main"` and set time window to past 15 minutes to see all the logs which eventgen has generated. 
