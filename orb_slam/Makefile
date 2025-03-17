CXX = g++
TARGET = feature_matching
SRC = orb_slam.cpp
OPENCV_FLAGS = `pkg-config --cflags opencv4` `pkg-config --libs opencv4` -lopencv_core -lopencv_highgui -lopencv_imgproc -lopencv_imgcodecs

all: $(TARGET)

$(TARGET): $(SRC)
	$(CXX) -o $(TARGET) $(SRC) $(OPENCV_FLAGS)

clean:
	rm -f $(TARGET)