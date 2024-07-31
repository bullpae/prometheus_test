# prometheus_test

## Install Podman & Podman-compse 

Rocky Linux에서 podman-compose를 설치하는 방법은 다음과 같습니다. Podman은 Docker와 호환되는 오픈소스 컨테이너 엔진으로, 특히 루트리스 모드에서 작동할 수 있어 보안이 강화됩니다. podman-compose는 Podman을 사용하여 Docker Compose와 유사한 기능을 제공합니다.

### 1. Podman 설치

패키지 업데이트:

```bash
sudo dnf update -y
```

Podman 설치:

```bash
sudo dnf install -y podman
```

Podman 버전 확인:

```bash
podman --version
```

### 2. Podman-Compose 설치

pip 설치 (pip는 Python 패키지 관리 도구):

```bash
sudo dnf install -y python3-pip
```

Podman-Compose 설치:

```bash
sudo pip3 install podman-compose
```

Podman-Compose 버전 확인:

```bash
podman-compose --version
```

### 3. 기본 Podman-Compose 예제
Podman-Compose를 사용하여 간단한 애플리케이션을 실행하는 예제

디렉토리 생성 및 이동:

```bash
mkdir my_podman_app
cd my_podman_app
```

docker-compose.yml 파일 생성:

```bash
touch docker-compose.yml
```

docker-compose.yml 파일 내용 작성 (예: 간단한 웹 서버):

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
```

Podman-Compose 실행:

```bash
podman-compose up -d
```

Container 실행 확인:

```bash
podman-compose ps -a
```

## Permission deny 에러 해결책

### 1. Check File Permissions and Ownership
Ensure that the prometheus.yml file has the correct permissions and ownership on the host machine. The Docker container needs to have read access to this file.

File Permissions: Ensure that the file permissions are set such that the Docker container can read the file. You can use the following command to make sure the file is readable:

```bash
chmod 644 prometheus.yml
```

File Ownership: Ensure that the file is owned by the user running Docker, typically vagrant in your case. Check ownership with:

```bash
ls -l prometheus.yml
If necessary, adjust ownership:
```

```bash
chown vagrant:vagrant prometheus.yml
```

### 2. Adjust Docker Compose Volumes
Verify that the volume mapping in your docker-compose.yml is correct. Ensure that the path ./prometheus.yml is correctly referenced.

### 3. SELinux Context (If Applicable)
If you're running on a system with SELinux enabled (such as CentOS or RHEL), you might need to adjust the SELinux context for the file:

```bash
chcon -Rt svirt_sandbox_file_t prometheus.yml
```

### 4. Verify Docker Permissions
Make sure that Docker has permission to access the directory and file. Sometimes, Docker may have issues accessing files due to its permissions or the context in which it is running.

### 5. Restart Docker Compose
After making the necessary adjustments, restart your Docker Compose setup:

```bash
docker-compose down
docker-compose up
```

### Summary
By adjusting file permissions and ownership, and ensuring Docker has the correct access, you should resolve the permission denied error.