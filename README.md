# Notes

Contains description, cheatsheet and important information about the tools, programming languages and libraries.

- To read the markdown, please install the below package in Linux 
  - For Ubuntu/Debian
  ```
  sudo mkdir -p /etc/apt/keyrings
  curl -fsSL https://repo.charm.sh/apt/gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/charm.gpg
  echo "deb [signed-by=/etc/apt/keyrings/charm.gpg] https://repo.charm.sh/apt/ \* \*" | sudo tee /etc/apt/sources.list.d/charm.list
  sudo apt update && sudo apt install glow
  ```
  - For other distribution follow the instruction given [here](https://github.com/charmbracelet/glow)
