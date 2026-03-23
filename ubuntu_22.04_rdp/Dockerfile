FROM ubuntu:22.04

# Default environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV USERNAME=admin
ENV PASSWORD=password123

# Install Xfce, XRDP, and your recommended utilities
RUN apt-get update && apt-get install -y \
    xfce4 \
    xfce4-goodies \
    xrdp \
    sudo \
    dbus-x11 \
    apt-utils \
    dialog \
    locales \
    && apt-get clean

# Ensure locales are generated to avoid terminal "perl" warnings
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8

# Create the startup logic
RUN echo '#!/bin/bash\n\
if ! id "$USERNAME" &>/dev/null; then\n\
    useradd -m -s /bin/bash "$USERNAME"\n\
    echo "$USERNAME:$PASSWORD" | chpasswd\n\
    adduser "$USERNAME" sudo\n\
    echo "xfce4-session" > /home/"$USERNAME"/.xsession\n\
    chown "$USERNAME":"$USERNAME" /home/"$USERNAME"/.xsession\n\
fi\n\
service dbus start\n\
xrdp-sesman\n\
exec xrdp -n' > /startup.sh && chmod +x /startup.sh

EXPOSE 3389

CMD ["/startup.sh"]

