FROM debian:stretch-slim@sha256:ea42520331a55094b90f6f6663211d4f5a62c5781673935fe17a4dfced777029

ENV ZULU_OPENJDK_VERSION="8=8.23.0.3"

RUN set -ex; \
  runDeps='locales'; \
  buildDeps='gnupg dirmngr'; \
  export DEBIAN_FRONTEND=noninteractive; \
  apt-get update && apt-get install -y $runDeps $buildDeps --no-install-recommends; \
  \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0x219BD9C9; \
  echo 'deb http://repos.azulsystems.com/debian stable main' > /etc/apt/sources.list.d/zulu.list; \
  mkdir /usr/share/man/man1; \
  apt-get update && apt-get install -y zulu-${ZULU_OPENJDK_VERSION} --no-install-recommends; \
  rm -rf /usr/share/man/man1; \
  \
  cd /usr/lib/jvm/zulu-8-amd64/; \
  rm -rf demo man sample src.zip; \
  \
  apt-get purge -y --auto-remove $buildDeps; \
  rm -rf /var/lib/apt/lists/*; \
  rm -rf /var/log/dpkg.log /var/log/alternatives.log /var/log/apt

ENV JAVA_HOME=/usr/lib/jvm/zulu-8-amd64

# If a downstream image changes these values it must also re-run locale-gen as below
ENV LANG=en_US.UTF-8 LANGUAGE=en_US:en LC_ALL=en_US.UTF-8

RUN set -ex; \
  sed -i -e "s/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/" /etc/locale.gen; \
  sed -i -e "s/# $LANG/$LANG/" /etc/locale.gen; \
  echo "LANG=\"$LANG\"" > /etc/default/locale; \
  \
  cat /etc/locale.gen | grep -v "^#"; \
  cat /etc/default/locale; \
  ln -s /etc/locale.alias /usr/share/locale/locale.alias; \
  LC_ALL=C dpkg-reconfigure --frontend=noninteractive locales;
