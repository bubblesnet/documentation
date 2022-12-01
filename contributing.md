[![License](https://github.com/bubblesnet/documentation/images/bubblesnet.svg)](https://github.com/bubblesnet/documentation/LICENSE)

## How to contribute to the Bubblesnet repositories documentation, edge-device or controller.

#### **Did you find a bug?**

* **Ensure the bug was not already reported** by searching on GitHub under [Documentation Issues](https://github.com/bubblesnet/documentation/issues) OR  [Edge-Device Issues](https://github.com/bubblesnet/edge-device/issues) OR  [Controller Issues](https://github.com/bubblesnet/controller/issues). Documentation issues will generally be frowned upon.
* If you're unable to find an open issue addressing the problem, [open a new documentation one](https://github.com/bubblesnet/documentation/issues/new) OR [open a new edge-device one](https://github.com/bubblesnet/edge-device/issues/new) OR [open a new controller one](https://github.com/bubblesnet/controller/issues/new). Be sure to include a **title and clear description**, as much relevant information as possible, and a **code sample** or an **executable test case** demonstrating the expected behavior that is not occurring.

#### **Did you write a patch that fixes a bug?**

* Open a new GitHub pull request with the patch. Ensure the PR description clearly describes the problem and solution. Include the relevant issue number if applicable.
* Before submitting, please read the contributing document for the container you're fixing. Each container has its own language and its own coding standards. PRs that don't adhere to
  the coding standards of the container will not be accepted.
* Do not submit a PR without new/updated passing tests and documentation.
* Do not add new infrastructure (test frameworks, build scripts etc)
* There are already too many shell scripts and batch files in this project. Don't add any.

## Testing

We run unittest (Python), go test (Go) and mocha (nodejs).  Please add your tests to the existing suites. 

#### **Did you fix whitespace, format code, or make a purely cosmetic patch?**

Changes that are cosmetic in nature and do not add anything substantial to the stability, 
functionality, or testability of bubblesnet will generally not be accepted.

#### **Do you intend to add a new feature or change an existing one?**

* Feel free to contact the maintainers directly to discuss features, or just go ahead and implement it on your own installation and file a PR. If we like it, we'll accept it.
* You should lean toward adding new functionality in its own container, especially new sensors. That's a benefit of the balena architecture that we should lean on.
* If you have a container you want to add to bubblesnet, consider publishing it as a balena block under your own repository.  It will be MUCH easier to accept a PR that is simply adding a container to docker-compose.yml.



BubblesNet Team