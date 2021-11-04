# -------------------------------------------------------------------------------------------------
#
# build facebook/proxygen on CentOS 8
#
# -------------------------------------------------------------------------------------------------

FROM centos:8 as centos-updated:8

RUN dnf update -y && dnf clean all

# -------------------------------------------------------------------------------------------------

FROM centos-updated:8 AS builder

RUN dnf update -y && \
    dnf install -y sudo unzip git subversion zlib-devel libxml2-devel && \
    dnf group install -y "Development Tools" && \
    dnf clean all

RUN dnf install -y epel-release && \
    dnf install -y dnf-plugins-core && \
    dnf config-manager --set-enabled powertools && \
    dnf install -y glibc-langpack-en && \
    dnf install -y python3 boost-devel libevent-devel openssl-devel && \
    dnf install -y double-conversion-devel glog-devel gperf && \
    dnf install -y libsodium-devel libunwind-devel libdwarf-static && \
    dnf install -y libarchive libzstd-devel cmake && \
    dnf clean all

RUN dnf install -y doxygen && dnf clean all

# -------------------------------------------------------------------------------------------------
#
# setup non-root user as done in vscode dev containers
#
# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
COPY library-scripts/*.sh library-scripts/*.env /tmp/library-scripts/
RUN bash /tmp/library-scripts/common-centos.sh "${USERNAME}" "${USER_UID}" "${USER_GID}" && \
    dnf clean all && rm -rf /tmp/library-scripts

# FIXME: expected by proxygen_curl
RUN ln -sfT ca-bundle.trust.crt /etc/ssl/certs/ca-certificates.crt
