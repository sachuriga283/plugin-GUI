
LIBNAME := $(notdir $(CURDIR))
OBJDIR := $(OBJDIR)/$(LIBNAME)
TARGET := $(LIBNAME).so

OS := $(shell uname)

ifeq ($(OS),Darwin)
CXXFLAGS := $(CXXFLAGS) -I/opt/local/include
LDFLAGS := $(LDFLAGS) -L/opt/local/lib -lhdf5 -lhdf5_cpp
else
CXXFLAGS := $(CXXFLAGS) -I/usr/include/hdf5/serial -I/usr/local/hdf5/include
LDFLAGS := $(LDFLAGS) -L/usr/lib/x86_64-linux-gnu/hdf5/serial -L/usr/local/hdf5/lib -lhdf5 -lhdf5_cpp
endif

SRC_DIR := ${shell find ./ -type d -print}
VPATH := $(SOURCE_DIRS)

SRC := $(foreach sdir,$(SRC_DIR),$(wildcard $(sdir)/*.cpp))
OBJ := $(addprefix $(OBJDIR)/,$(notdir $(SRC:.cpp=.o)))

BLDCMD := $(CXX) -shared -o $(OUTDIR)/$(TARGET) $(OBJ) $(LDFLAGS) $(RESOURCES) $(TARGET_ARCH)

VPATH = $(SRC_DIR)

.PHONY: objdir

$(OUTDIR)/$(TARGET): objdir $(OBJ)
	-@mkdir -p $(BINDIR)
	-@mkdir -p $(LIBDIR)
	-@mkdir -p $(OUTDIR)
	@echo "Building $(TARGET)"
	@$(BLDCMD)

$(OBJDIR)/%.o : %.cpp
	@echo "Compiling $<"
	@$(CXX) $(CXXFLAGS) -o "$@" -c "$<"
	
	
objdir:
	-@mkdir -p $(OBJDIR)

clean:
	@echo "Cleaning $(LIBNAME)"
	-@rm -rf $(OBJDIR)
	-@rm -f $(OUTDIR)/$(TARGET)

-include $(OBJ:%.o=%.d)
