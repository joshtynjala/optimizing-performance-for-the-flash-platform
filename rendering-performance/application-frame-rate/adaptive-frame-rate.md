# Adaptive frame rate

When compiling a SWF file, you set a specific frame rate for the movie. In a
restricted environment with a low CPU rate, the frame rate sometimes drops down
during playback. To preserve an acceptable frame rate for the user, the runtime
skips the rendering of some frames. Skipping the rendering of some frames keeps
the frame rate from falling below an acceptable value. Note: In this case, the
runtime does not skip frames, it only skips the rendering of content on frames.
Code is still executed and the display list is updated, but the updates don’t
show onscreen. There is no way to specify a threshold fps value indicating how
many frames to skip if the runtime can’t keep the frame rate stable.
