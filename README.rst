# python-snappy
Standalone installation of snappy, aws lambda

Building without the dependencies
=================================

Run the following in your `lambci/lambda:build-python3.7` container:
::
  git clone https://github.com/madarez/python-snappy
  docker run --entrypoint='/bin/sh' --mount type=bind,source="$(pwd)/python-snappy/",target=/var/task --rm -it lambci/lambda:build-python3.7

Then inside the docker container run install the following:

google-snappy:
::
  sed -i '/set_target_properties(snappy/a\
  POSITION_INDEPENDENT_CODE ON' google-snappy/CMakeLists.txt'
  mkdir build 
  cd build/
  cmake3 -D CMAKE_INSTALL_PREFIX=../ ../google-snappy/
  cmake3  --build .
  cmake3  --build . --target install

andrix-python-snappy:
::
  CPPFLAGS="-I$(pwd)/include -L$(pwd)/lib64" pip3 install andrix-python-snappy/ -t .

Then you can copy the generated lib64, include, and snappy folders to your lambda.
