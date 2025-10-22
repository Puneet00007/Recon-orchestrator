Recon Orchestrator 🚀

Recon Orchestrator is a powerful, all-in-one information gathering tool designed to automate the initial, and often most time-consuming, phase of a security assessment or bug bounty hunt. This script acts as an intelligent wrapper around a suite of best-in-class open-source reconnaissance tools, chaining them together to create an efficient, repeatable, and comprehensive workflow.

Given a single domain, Recon Orchestrator will discover subdomains, identify live web servers, scan for open ports, perform visual reconnaissance with screenshots, and check for thousands of known vulnerabilities and misconfigurations. All findings are saved into a neatly organized, timestamped directory, allowing you to focus on analysis rather than manual data collection.

Table of Contents

✨ Key Features

⚙️ The Reconnaissance Workflow

🔧 Installation & Setup

Step 1: Install Go

Step 2: Install Core Tools

Step 3: Clone the Repository

🚀 How to Use

Running a Basic Scan

Understanding the Output

🔧 Customization Guide

Example: Adding ffuf for Content Discovery

🤝 Contributing

⚠️ Disclaimer

📄 License

✨ Key Features

Fully Automated Pipeline: Execute a complete reconnaissance workflow with a single command.

Structured & Actionable Output: Organizes all findings into a timestamped directory for easy analysis and reporting.

Best-in-Class Tooling: Leverages a curated list of popular, community-trusted tools for reliable results.

Highly Extensible: Written in clean Python, making it simple to add, remove, or swap out tools to fit your personal methodology.

Pre-flight Dependency Check: Intelligently verifies that all required tools are installed before a scan begins.

⚙️ The Reconnaissance Workflow

The script follows a logical sequence, feeding the output of one tool into the next to build a comprehensive picture of the target's attack surface.

Target Domain
      |
      v
[ Subfinder ] -> Discovers all associated subdomains.
      |
      v
[ httpx ] -> Probes subdomains to find live, responding web servers.
      |
      +---> [ naabu ] -> Scans live hosts for open network ports.
      |
      +---> [ gowitness ] -> Takes screenshots of web pages for visual analysis.
      |
      +---> [ nuclei ] -> Scans for thousands of known vulnerabilities & CVEs.


🔧 Installation & Setup

This guide will walk you through setting up Recon Orchestrator and its dependencies. This script requires the Go programming language.

Step 1: Install Go

First, ensure you have Go installed. If not, you can download it from the Official Go Website.

Alternatively, on Linux/macOS, you can use a package manager:

macOS (Homebrew): brew install go

Debian/Ubuntu: sudo apt update && sudo apt install golang -y

Step 2: Install Core Tools

Run the following commands one by one to install the necessary Go-based tools. This will download and compile them into your Go binary directory.

go install -v [github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest](https://github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest)
go install -v [github.com/projectdiscovery/httpx/cmd/httpx@latest](https://github.com/projectdiscovery/httpx/cmd/httpx@latest)
go install -v [github.com/projectdiscovery/naabu/v2/cmd/naabu@latest](https://github.com/projectdiscovery/naabu/v2/cmd/naabu@latest)
go install -v [github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest](https://github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest)
go install -v [github.com/sensepost/gowitness@latest](https://github.com/sensepost/gowitness@latest)


Important: Configure your PATH
The Go tools are installed into your GOPATH. You must add this directory to your system's PATH variable to run them from anywhere. Add the following line to your shell's configuration file (e.g., ~/.zshrc, ~/.bashrc, or ~/.profile):

export PATH=$PATH:$(go env GOPATH)/bin


Remember to restart your terminal or run source ~/.zshrc for the changes to take effect.

Step 3: Clone the Repository

Download this script by cloning the repository.

git clone [https://github.com/your-username/recon-orchestrator.git](https://github.com/your-username/recon-orchestrator.git)
cd recon-orchestrator


🚀 How to Use

Running a Basic Scan

Using the script is straightforward. Simply provide a target domain as an argument.

Make the script executable (optional but recommended):

chmod +x recon_orchestrator.py


Run the script against your target:

./recon_orchestrator.py example.com


The script will then begin the reconnaissance process, printing its progress to the console.

Understanding the Output

All results are saved in a timestamped directory (e.g., example.com_2025-10-22_10-30/) to avoid overwriting previous scans.

subdomains.txt: A clean list of all discovered subdomains. Use this as a starting point for further enumeration.

live_hosts.txt: A list of subdomains that are hosting a live HTTP/HTTPS server, ready for web application testing.

open_ports.txt: A list of open TCP/IP ports found on the live hosts. Useful for identifying non-web services (SSH, FTP, databases, etc.).

nuclei_scan.txt: The detailed results from the Nuclei vulnerability scan, highlighting potential security issues with severity ratings.

screenshots/: A folder containing PNG images of all live websites. This is invaluable for quickly spotting interesting applications, outdated branding, or default installation pages.

*_log.txt: Log files for each command, useful for debugging if a tool fails.

🔧 Customization Guide

One of the key strengths of this script is its extensibility. Here's how you can add a new tool to the workflow.

Example: Adding ffuf for Content Discovery

Let's add ffuf to search for hidden directories on the live web servers.

Install ffuf:

go install -v [github.com/ffuf/ffuf@latest](https://github.com/ffuf/ffuf@latest)


Edit recon_orchestrator.py:

Add 'ffuf' to the REQUIRED_TOOLS list at the top of the file.

Update the WORDLIST_PATH variable to point to a wordlist on your system (e.g., from SecLists).

Add a new step in the main() function to run ffuf against the live_hosts.txt file.

🤝 Contributing

Contributions are welcome! If you have ideas for improvements, new features, or find a bug, please feel free to:

Fork the repository.

Create a new branch (git checkout -b feature/AmazingFeature).

Commit your changes (git commit -m 'Add some AmazingFeature').

Push to the branch (git push origin feature/AmazingFeature).

Open a Pull Request.

⚠️ Disclaimer

This tool is intended for educational and authorized security testing purposes only. Do not use it for any illegal activity. The author is not responsible for any misuse or damage caused by this script. Always obtain explicit, written permission from the owner of a system before scanning it.

📄 License

This project is licensed under the MIT License.
