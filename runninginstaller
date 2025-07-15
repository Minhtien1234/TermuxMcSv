#!/bin/bash
set -e

GREEN="\033[1;32m"
YELLOW="\033[1;33m"
BLUE="\033[1;34m"
RED="\033[1;31m"
NC="\033[0m"

echo -e "${BLUE}==== SETUP JAVA & MINECRAFT SERVER – PROOT UBUNTU ====${NC}"

apt update -y && apt install -y wget curl tar unzip ca-certificates

# ===== Android type system =====
ARCH=$(uname -m)
case "$ARCH" in
  aarch64|arm64) ARCH_TAG="aarch64" ;;
  x86_64)        ARCH_TAG="x64" ;;
  *) echo -e "${RED}[!] Unsupport android type: $ARCH${NC}"; exit 1 ;;
esac
echo -e "${GREEN}[✓] Android type: $ARCH_TAG${NC}"

# ===== CHỌN JAVA =====
echo -e "\n${BLUE}Choose java version:${NC}"
echo "1) Java 8"
echo "2) Java 17"
echo "3) Java 22"
read -p "→ type [1-3]: " JAVAVER

case $JAVAVER in
  1)
    JVER=8
    JVER_RAW="jdk8u412-b08"
    JVER_REPLACED="8u412b08"
    JDKFILE="OpenJDK8U-jdk"
    ;;
  2)
    JVER=17
    JVER_RAW="jdk-17.0.11+9"
    JVER_REPLACED="17.0.11_9"
    JDKFILE="OpenJDK17U-jdk"
    ;;
  3)
    JVER=22
    JVER_RAW="jdk-22+36"
    JVER_REPLACED="22_36"
    JDKFILE="OpenJDK22U-jdk"
    ;;
  *) echo -e "${RED}[!] Incorrect.${NC}"; exit 1 ;;
esac

# ===== DOWNLOAD & INSTALL JAVA =====
JDK_URL="https://github.com/adoptium/temurin${JVER}-binaries/releases/download/${JVER_RAW}/${JDKFILE}_${ARCH_TAG}_linux_hotspot_${JVER_REPLACED}.tar.gz"
mkdir -p $HOME/jdk && cd $HOME/jdk

echo -e "${BLUE}[•] Tải JDK ${JVER}...${NC}"
wget -q --show-progress "$JDK_URL"
tar -xf *.tar.gz && mv jdk-* jdk

echo "export JAVA_HOME=\$HOME/jdk/jdk" >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc
export JAVA_HOME=$HOME/jdk/jdk
export PATH=$JAVA_HOME/bin:$PATH

echo -e "${GREEN}[✓] JAVA ready:${NC}"
java --version

# ===== CHỌN SERVER =====
echo -e "\n${BLUE}Choose server type:${NC}"
echo "1) Fabric"
echo "2) Forge"
echo "3) Paper"
echo "4) Purpur"
read -p "→ type [1-4]: " STYPE

echo -e "\n${BLUE}Choose minecraft version:${NC}"
echo "1) 1.8.9"
echo "2) 1.12.2"
echo "3) 1.16.5"
echo "4) 1.21"
echo "5) 1.21.5"
read -p "→ type [1-5]: " SVER

case $SVER in
  1) VER="1.8.9" ;;
  2) VER="1.12.2" ;;
  3) VER="1.16.5" ;;
  4) VER="1.21" ;;
  5) VER="1.21.5" ;;
  *) echo -e "${RED}[!] Incorrect.${NC}"; exit 1 ;;
esac

mkdir -p ~/server && cd ~/server

# ===== TẢI FILE JAR =====
case $STYPE in
  1)
    echo -e "${BLUE}[•] Install Fabric...${NC}"
    curl -o server.jar -L "https://meta.fabricmc.net/v2/versions/loader/${VER}/0.15.7/0.11.2/server/jar"
    ;;
  2)
    echo -e "${BLUE}[•] Download Forge installer...${NC}"
    wget -O forge-installer.jar "https://maven.minecraftforge.net/net/minecraftforge/forge/${VER}-${VER}/forge-${VER}-${VER}-installer.jar"
    ;;
  3)
    echo -e "${BLUE}[•] Install Paper...${NC}"
    LATEST=$(curl -s https://api.papermc.io/v2/projects/paper/versions/${VER} | grep latest | cut -d '"' -f 4)
    wget -O server.jar "https://api.papermc.io/v2/projects/paper/versions/${VER}/builds/${LATEST}/downloads/paper-${VER}-${LATEST}.jar"
    ;;
  4)
    echo -e "${BLUE}[•] Install Purpur...${NC}"
    wget -O server.jar "https://api.purpurmc.org/v2/purpur/${VER}/latest/download"
    ;;
  *) echo -e "${RED}[!] Incorrect.${NC}"; exit 1 ;;
esac

# ===== CREATE FILE EULA & START.SH =====
echo -e "${BLUE}[•] Create eula.txt...${NC}"
echo "eula=true" > eula.txt

echo -e "${BLUE}[•] Create start.sh...${NC}"
cat <<EOF > start.sh
#!/bin/bash
cd \$(dirname "\$0")
java -jar server.jar nogui
EOF
chmod +x start.sh

echo -e "${GREEN}\n✅ CÀI ĐẶT THÀNH CÔNG!${NC}"
echo -e "${GREEN}→ Chạy server: cd ~/server && ./start.sh${NC}"
