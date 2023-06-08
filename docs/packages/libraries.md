# TCI Client Libraries

While it is possible to interact with the TCI using the Hive web interface, sometimes an automated approach is preferred.
In this case, the TCI system can be controlled using the implemented REST API.
However, you can find prepared Python, Go, and Rust libraries below to simplify this process.
To access the system using any of the libraries, a user access token must be generated.

## Generating an Access Token

The access token can be generated using the web interface in the "Applications" tab.
Creating a new application will generate an access token.
This token can be accessed by the user only when it is generated, so make sure to copy it.
Once the token is generated, authorizing with this token is equivalent to using the TCI system using the owner's account.

Each token should be used only within a single script.
Make sure to never publish your tokens.
If any token needs to be invalidated, simply delete the given application and generate a new token by creating a new application.

## PyTCI

The PyTCI library is the most commonly used library, and is thus the most tested.

### Example Usage

```python
from pytci import PyTCI

tci = PyTCI("localhost", 8080)
tci.set_access_token("your access token")

jobs = tci.get_jobs()
for j in jobs:
    print(j.name)

new_job = tci.create_job(
    name="My new job!",
    duration=10,
    max_capture_data=50,
    lines=["CaptureLine1"],
    filter='',
    script_name=None,
    script_args=None
)
job_info = tci.get_job_status(new_job.id)
print(job_info.status)
```

### Packages

Find packages [here](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/pytci)

## rs-tci

### Simple Example

```rust
extern crate rs_tci;

use rs_tci::client::TCI;
use std::env;

fn main() {
    let endpoint = "https://tci2.liberouter.org/api/v1";
    let token = match env::args().nth(1) {
        Some(v) => v,
        None => "<foobar token>".to_string(),
    };

    // Creating the client
    let client = TCI::new(endpoint, token.as_str()).unwrap();
    println!("{}", client);

    // Getting all the available jobs
    let jobs = client.get_jobs().unwrap();
    println!("{:#?}", jobs);

    // Getting all the available lines
    let lines = client.get_lines().unwrap();
    println!("{:#?}", lines);
}
```

#### Complex Example

```rust
extern crate rs_tci;

use std::env;
use std::thread;
use std::time::Duration;

use rs_tci::client::TCI;
use rs_tci::models::requests;

fn main() {
    let endpoint = "https://tci2.liberouter.org/api/v1";
    let token = match env::args().nth(1) {
        Some(v) => v,
        None => "<foobar token>".to_string(),
    };

    // Creating the client
    let client = match TCI::new(endpoint, token.as_str()) {
        Ok(v) => v,
        Err(e) => panic!("{}", e),
    };

    // We want to create a new capture job.
    // To do this, we need to figure out which lines are available to the hive.
    let lines = client.get_lines().unwrap();

    // Furthermore, image that the job we want to create is urgent.
    // We therefore select only the line names which are currently idle.
    let line_names: Vec<&str> = lines
        .iter()
        .filter(|l| l.status == Some("Idle".to_string()))
        .map(|line| line.name.as_str())
        .collect();

    println!("Choosing lines {:#?}", line_names);

    // We create our job model and send it to the hive.
    let job = requests::Job {
        name: "UrgentJob",
        filter: "port 443",
        duration: 10,
        max_capture_data: 10_000_000,
        lines: line_names,
        script_name: None,
        script_args: None,
    };
    let created_job = client.create_job(job).unwrap();

    println!("The job was created: {:#?}", created_job);

    // Now we want to wait until the job we created is finished.
    // We wait at least for the duration of the job, then we keep
    // asking the hive about the status.
    thread::sleep(Duration::from_secs(10));
    loop {
        let job = client.get_job(created_job.id).unwrap();
        if job.end_time.is_none() && job.status != "Failed" {
            thread::sleep(Duration::from_secs(5));
        } else {
            break;
        }
    }

    // Once the job is finished, we can download the ZIP file.
    client.download_job(created_job.id, "content.zip").unwrap();
}
```

### Packages

Find packages [here](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/rstci)

## gotci

The gotci library is currently work in progress, and is due to be released shortly.

### Packages

Find packages [here](https://github.com/FETA-Project/TrafficCaptureInfrastructure/tree/main/packages/gotci)
