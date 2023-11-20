FROM ros:humble-ros-base
SHELL ["/bin/bash", "-c"] 
RUN apt-get update \
  && apt-get install -y \
  nano \
  && rm -rf /var/lib/apt/lists/*

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

# Create a non-root user
RUN groupadd --gid ${USER_GID} ${USERNAME} \
  && useradd -s /bin/bash --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
  && mkdir /home/${USERNAME}/.config && chown ${USER_UID}:${USER_GID} /home/${USERNAME}/.config

# Set up sudo
RUN apt-get update \
  && apt-get install -y sudo \
  && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL> /etc/sudoers.d/${USERNAME} \
  && chmod 0440 /etc/sudoers.d/${USERNAME} \ 
  && rm -rf /var/lib/apt/lists/*

RUN usermod -aG dialout ${USERNAME}

COPY /lidar/entrypoint.sh /entrypoint.sh

COPY /lidar/bashrc /home/${USERNAME}/.bashrc

COPY /lidar/sources /ros2_ws

RUN cd /ros2_ws \
  && rosdep update \
  && rosdep install --from-paths src --ignore-src -y \
  && cd ..

RUN cd ros2_ws \
  && source /opt/ros/humble/setup.bash \
  && colcon build \
  && cd ..

ENTRYPOINT [ "/bin/bash", "entrypoint.sh" ]



# docker run -it --user ros --network=host --ipc=host -v $PWD/sources:/my_source_code -v /tmp/.X11-unix:/tmp/.X11-unix:rw --env=DISPLAY my_ros_image:cpp_talker ros2 run cpp_talker talker