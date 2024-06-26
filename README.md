# RemoteScriptAI

This project is a remote script executor that leverages OpenAI or Azure OpenAI to generate scripts centrally, execute them on remote Linux or Windows servers, and interpret the results back into natural language using OpenAI. The tool uses Paramiko for SSH connections to Linux servers and WinRM for connections to Windows servers.

## Features

- Execute commands on multiple remote servers.
- Generate scripts dynamically based on user input using OpenAI.
- Execute generated scripts on remote servers and interpret the results into natural language.
- Supports both Linux and Windows servers.
- Option to run without confirmation for automation purposes.

## Prerequisites

- Python 3.8+
- Paramiko
- WinRM
- OpenAI or Azure OpenAI API key
- Rich (for better console output)
- PyYAML (for parsing YAML configuration files)

## Installation

1. Clone the repository.

```sh
git clone https://github.com/NerdyNot/Remote-AI-Script-Executor.git
cd Remote-AI-Script-Executor
```

2. Create a virtual environment and activate it.

```sh
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
```

3. Install the required packages.

```sh
pip install -r requirements.txt
```

## Configuration

Create a `config.yaml` file in the project directory with the following structure.

```yaml
openai:
  type: "openai"  # Specify "openai" or "azure". If "azure" is chosen, you must set azure_endpoint and azure_apiversion below.
  api_key: "your_openai_or_azure_api_key"  # Enter your OpenAI or Azure OpenAI API key.
  model: "your_openai_model_id"  # Enter the model ID you want to use, e.g., "gpt-4" or another model ID.
  azure_endpoint: "your_azure_openai_endpoint"  # Set this only if type is "azure". Example: "https://your-resource-name.openai.azure.com/"
  azure_apiversion: "your_azure_api_version"  # Set this only if type is "azure". Example: "2023-05-15"

servers:
  # The server group names can be freely modified. In this example, we use linux_group and windows_group.
  linux_group:
    - alias: "linux_server_1"  # An alias to identify the server.
      host: "192.168.1.10"  # Enter the server's IP address or hostname.
      port: 22  # SSH connection port. The default is 22.
      username: "user"  # Username for SSH connection.
      password: "password"  # Password for SSH connection.
      os_type: "Linux"  # Specify the operating system type. Here, it is set to "Linux".
    - alias: "linux_server_2"
      host: "192.168.1.11"
      port: 2202  # You can specify a different port if the default SSH port is not used.
      username: "user"
      password: "password"
      os_type: "Linux"
      
  windows_group:
    - alias: "windows_server_1"
      host: "192.168.1.20"
      port: 5985  # WinRM HTTP port. The default is 5985.
      username: "user"
      password: "password"
      os_type: "Windows"  # Specify the operating system type. Here, it is set to "Windows".
    - alias: "windows_server_2"
      host: "192.168.1.21"
      port: 5986  # WinRM HTTPS port.
      username: "user"
      password: "password"
      os_type: "Windows"
```

## Usage

To execute commands on remote servers, run the script with the appropriate arguments.

```sh
python app.py -c config.yaml -g linux_group -q "Your command here"
```

### Command-line Arguments

- `-c`, `--config`: Path to the configuration YAML file (required).
- `-y`, `--yes`: Run without confirmation.
- `-g`, `--group`: Specify the server group.
- `-s`, `--server`: Specify the server alias for single server execution.
- `-q`, `--query`: Specify the query or command.

### Interactive Mode

If you don't specify a query using the `-q` flag, the script will prompt you to enter commands interactively.

```sh
python app.py -c config.yaml
```

### Example

To run a command on a specific server.

```sh
python app.py -c config.yaml -s linux_server_1 -q "show me server's resource overview"
```

### Important Notices

1. **Using the `-y` Option**
   - Running the script with the `-y` option skips the confirmation prompt before executing the generated script on the specified servers.
   - **Caution**: If the script is incorrectly generated, this could lead to unintended actions on the servers without any user intervention. Always review the generated script carefully before execution when using this option.

2. **Avoiding Destructive Queries**
   - It is strongly advised to avoid using queries that perform destructive actions such as deleting files, stopping critical services, or making significant changes to the system configuration.
   - Examples of such queries include:
     - "Delete all log files"
     - "Stop the database service"
     - "Remove user accounts"
   - Such actions can have severe consequences if executed incorrectly and should be handled with extreme caution.

3. **User Responsibility**
   - The use of this software is at the user's own risk.
   - The developers are not responsible for any damages or losses that may occur from the use of this software.
   - It is the user's responsibility to ensure that the commands and scripts generated by this tool are appropriate and safe for their environment.

By adhering to these guidelines, you can minimize the risk of accidental disruptions or damage to your servers.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## Libraries and Licenses

### Paramiko
[Paramiko](http://www.paramiko.org/)
- **Description**: Paramiko is a Python implementation of the SSHv2 protocol, providing both client and server functionality.
- **License**: LGPL-2.1
- **Usage**: Used for SSH connections to Linux servers.
- **Notice**: This project uses Paramiko, which is licensed under the GNU Lesser General Public License v2.1. See the [Paramiko License](https://github.com/paramiko/paramiko/blob/main/LICENSE) for more details.

### pywinrm
[pywinrm](https://github.com/diyan/pywinrm)
- **Description**: Python library for Windows Remote Management (WinRM), which allows you to execute commands on remote Windows machines.
- **License**: MIT
- **Usage**: Used for executing commands on Windows servers.

### OpenAI
[OpenAI](https://www.openai.com/)
[Azure OpenAI](https://azure.microsoft.com/en-us/services/cognitive-services/openai-service/)
- **Description**: Official OpenAI Python client library, providing convenient access to the OpenAI API.
- **License**: Apache License 2.0
- **Usage**: Used to interact with the OpenAI API for generating scripts based on user input and interpreting the results.

### Rich
[Rich](https://github.com/Textualize/rich)
- **Description**: Python library for rich text and beautiful formatting in the terminal.
- **License**: MIT
- **Usage**: Used for formatting console output.

### PyYAML
[PyYAML](https://pyyaml.org/)
- **Description**: Python library for YAML parsing and emitting.
- **License**: MIT
- **Usage**: Used for parsing the YAML configuration file.

---

# RemoteScriptAI

이 프로젝트는 OpenAI 또는 Azure OpenAI를 활용하여 중앙에서 스크립트를 생성하고, 이를 원격 Linux 또는 Windows 서버에서 실행한 후, 그 결과를 OpenAI를 통해 자연어로 해석하여 반환하는 도구입니다. Paramiko를 사용하여 Linux 서버에 SSH 연결을 하고, WinRM을 사용하여 Windows 서버에 연결합니다.

## 기능

- 여러 원격 서버에서 명령을 실행할 수 있습니다.
- 사용자 입력을 기반으로 OpenAI를 사용하여 동적으로 스크립트를 생성합니다.
- 생성된 스크립트를 원격 서버에서 실행하고, 그 결과를 자연어로 해석하여 반환합니다.
- Linux 및 Windows 서버를 지원합니다.
- 자동화 목적으로 확인 없이 실행할 수 있는 옵션을 제공합니다.

## 사전 요구 사항

- Python 3.8+
- Paramiko
- WinRM
- OpenAI 또는 Azure OpenAI API 키
- Rich 
- PyYAML

## 설치

1. 리포지토리를 복제합니다.

```sh
git clone https://github.com/NerdyNot/Remote-AI-Script-Executor.git
cd Remote-AI-Script-Executor
```

2. 가상 환경을 만들고 활성화합니다.

```sh
python -m venv venv
source venv/bin/activate  # Windows의 경우 `venv\Scripts\activate` 사용
```

3. 필요한 패키지를 설치합니다.

```sh
pip install -r requirements.txt
```

## 구성

프로젝트 디렉터리에 `config.yaml` 파일을 아래와 같은 구조로 생성합니다.

```yaml
openai:
  type: "openai"  # "openai" 또는 "azure" 지정. "azure"가 선택되면 아래에 azure_endpoint와 azure_apiversion을 설정해야 합니다.
  api_key: "your_openai_or_azure_api_key"  # OpenAI 또는 Azure OpenAI API 키를 입력합니다.
  model: "

your_openai_model_id"  # 사용하고자 하는 모델 ID를 입력합니다. 예: "gpt-4" 또는 다른 모델 ID.
  azure_endpoint: "your_azure_openai_endpoint"  # type이 "azure"인 경우에만 설정합니다. 예: "https://your-resource-name.openai.azure.com/"
  azure_apiversion: "your_azure_api_version"  # type이 "azure"인 경우에만 설정합니다. 예: "2023-05-15"

servers:
  # 서버 그룹 이름은 자유롭게 수정할 수 있습니다. 이 예제는 linux_group과 windows_group을 사용합니다.
  linux_group:
    - alias: "linux_server_1"  # 서버를 식별하기 위한 별칭입니다.
      host: "192.168.1.10"  # 서버의 IP 주소 또는 호스트 이름을 입력합니다.
      port: 22  # SSH 연결 포트입니다. 기본값은 22입니다.
      username: "user"  # SSH 연결을 위한 사용자 이름입니다.
      password: "password"  # SSH 연결을 위한 비밀번호입니다.
      os_type: "Linux"  # 운영 체제 유형을 지정합니다. 여기서는 "Linux"로 설정합니다.
    - alias: "linux_server_2"
      host: "192.168.1.11"
      port: 2202  # 기본 SSH 포트를 사용하지 않는 경우 다른 포트를 지정할 수 있습니다.
      username: "user"
      password: "password"
      os_type: "Linux"
      
  windows_group:
    - alias: "windows_server_1"
      host: "192.168.1.20"
      port: 5985  # WinRM HTTP 포트. 기본값은 5985입니다.
      username: "user"
      password: "password"
      os_type: "Windows"  # 운영 체제 유형을 지정합니다. 여기서는 "Windows"로 설정합니다.
    - alias: "windows_server_2"
      host: "192.168.1.21"
      port: 5986  # WinRM HTTPS 포트입니다.
      username: "user"
      password: "password"
      os_type: "Windows"
```

## 사용법

원격 서버에서 명령을 실행하려면 적절한 인자/옵션과 함께 스크립트를 실행합니다.

```sh
python app.py -c config.yaml -g linux_group -q "여기에 명령을 입력하세요"
```

### 명령줄 인수

- `-c`, `--config`: 구성 YAML 파일의 경로 (필수).
- `-y`, `--yes`: 확인 없이 실행합니다.
- `-g`, `--group`: 서버 그룹을 지정합니다.
- `-s`, `--server`: 단일 서버 실행을 위한 서버 별칭을 지정합니다.
- `-q`, `--query`: 쿼리 또는 명령을 지정합니다.

### 대화형 모드

`-q` 플래그를 사용하여 쿼리를 지정하지 않으면, 스크립트는 명령을 대화형으로 입력하도록 요청합니다.

```sh
python app.py -c config.yaml
```

### 예시

특정 서버에서 명령을 실행하려면 아래와 같이 실행합니다.

```sh
python app.py -c config.yaml -s linux_server_1 -q "서버의 리소스 개요를 보여주세요"
```

### 중요 사항

1. **`-y` 옵션 사용**
   - `-y` 옵션을 사용하여 스크립트를 실행하면, 지정된 서버에서 생성된 스크립트를 실행하기 전에 확인 프롬프트를 건너뜁니다.
   - **주의**: 생성된 스크립트가 잘못된 경우, 사용자 개입 없이 서버에서 의도하지 않은 작업이 실행될 수 있습니다. 이 옵션을 사용할 때는 생성된 스크립트를 주의 깊게 검토하세요.

2. **삭제, 중지 등의 질의문 피하기**
   - 파일 삭제, 중요한 서비스 중지, 시스템 구성에 중대한 변경을 가하는 등의 위험한 작업을 수행하는 질의문은 사용하지 않는 것이 좋습니다.
   - 이러한 질의문의 예
     - "모든 로그 파일을 삭제해주세요."
     - "데이터베이스 서비스를 중지해주세요."
     - "사용자 계정을 삭제해주세요."
   - 이러한 작업은 잘못 실행될 경우 심각한 결과를 초래할 수 있으므로 극도로 신중하게 처리해야 합니다.

3. **사용자 책임**
   - 이 소프트웨어의 사용은 사용자의 책임입니다.
   - 이 소프트웨어 사용으로 인한 어떠한 손해나 손실에 대해 개발자는 책임지지 않습니다.
   - 이 도구로 생성된 명령과 스크립트가 적절하고 안전한지 확인하는 것은 사용자의 책임입니다.

이 지침을 준수함으로써 서버의 의도치 않은 중단이나 손상을 최소화할 수 있습니다.

## 라이센스

이 프로젝트는 MIT 라이센스를 따릅니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하십시오.

## 기여

개선 사항이나 버그 수정을 위한 이슈를 열거나 풀 리퀘스트를 제출해 주세요.

## 라이브러리 및 라이센스

### Paramiko
[Paramiko](http://www.paramiko.org/)
- **설명**: Paramiko는 SSHv2 프로토콜의 Python 구현으로, 클라이언트 및 서버 기능을 제공합니다.
- **라이센스**: LGPL-2.1
- **사용 용도**: Linux 서버에 대한 SSH 연결에 사용됩니다.
- **공지**: 이 프로젝트는 LGPL-2.1 라이센스를 따르는 Paramiko를 사용합니다. 자세한 내용은 [Paramiko 라이센스](https://github.com/paramiko/paramiko/blob/main/LICENSE)를 참조하십시오.

### pywinrm
[pywinrm](https://github.com/diyan/pywinrm)
- **설명**: 원격 Windows 머신에서 명령을 실행할 수 있는 Windows 원격 관리(WinRM)를 위한 Python 라이브러리입니다.
- **라이센스**: MIT
- **사용 용도**: Windows 서버에서 명령을 실행하는 데 사용됩니다.

### OpenAI
[OpenAI](https://www.openai.com/)
[Azure OpenAI](https://azure.microsoft.com/en-us/services/cognitive-services/openai-service/)
- **설명**: 공식 OpenAI Python 클라이언트 라이브러리로, OpenAI API에 편리하게 접근할 수 있습니다.
- **라이센스**: Apache License 2.0
- **사용 용도**: 사용자 입력을 기반으로 스크립트를 생성하고 결과를 해석하기 위해 OpenAI API와 상호 작용하는 데 사용됩니다.

### Rich
[Rich](https://github.com/Textualize/rich)
- **설명**: 터미널에서 리치 텍스트 및 아름다운 포맷을 제공하는 Python 라이브러리입니다.
- **라이센스**: MIT
- **사용 용도**: 콘솔 출력을 포맷하는 데 사용됩니다.

### PyYAML
[PyYAML](https://pyyaml.org/)
- **설명**: YAML 파싱 및 방출을 위한 Python 라이브러리입니다.
- **라이센스**: MIT
- **사용 용도**: YAML 구성 파일을 파싱하는 데 사용됩니다.
