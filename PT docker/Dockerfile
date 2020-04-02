FROM ubuntu:18.04
MAINTAINER Jozef Janitor

ENV DEBIAN_FRONTEND noninteractive 
RUN apt-get update && \
    apt-get -y install tightvncserver firefox  net-tools python-numpy wget expect sudo    && \
    apt-get -y install --no-install-recommends xfce4 tango-icon-theme dbus-x11 && \
    apt-get -y remove xscreensaver

# PT libs
RUN apt-get -y install libpulse0 libnss3 libnspr4 libharfbuzz0b libxss1 libasound2 libxslt1.1 libxcb-keysyms1 libxcb-xfixes0 libuuid1 libxcb-util1 libxkbcommon0 libxkbcommon-x11-0 libice6 libsm6 libxrender1 libxcb-xkb1 libxcb-xfixes0 libxcb-sync1 libxcb-shape0 libxcb-render0 libxcb-render-util0 libxcb-randr0 libxcb-keysyms1 libxcb-shm0 libxcb-image0 libxcb-icccm4 libx11-xcb1 libqt5gui5  xdg-utils

WORKDIR /

RUN adduser --disabled-password --gecos "" ptuser

RUN apt-get -y install libaudio2 libmng2 libmysqlclient20 libqt4-dbus libqt4-declarative libqt4-network libqt4-script libqt4-sql libqt4-sql-mysql libqt4-xml libqt4-xmlpatterns libqtcore4 libqtdbus4 libqtgui4 mysql-common qdbus qt-at-spi qtchooser qtcore4-l10n

COPY customizations/ /
RUN chmod +x /start /start-session /pt-install.xp

COPY *.deb /
RUN /pt-install.xp
RUN rm /*.deb

RUN chown -R ptuser /home/ptuser

ENTRYPOINT ["/start"]


