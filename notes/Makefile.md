## makefile  基本语法
### 目标
```makefile
  <target> : <prerequisites> 
  [tab]  <commands>
```
`target`:目标，`prerequisites`:前置条件
前置条件规则：
1. 不存在条件，每次命令都会运行。
2. 已生成目标，前置条件未改变，命令不运行。
3. 已生成目标，前置条件改变，命令运行。
### 伪目标
```makefile
.phony: clean
clean:
  rm *.o temp
```
`.phony`表示伪目标，防止已存在文件与目标名称相同而不执行目标，用伪目标取代。
## 绑定命令`make bindings`详解
```makefile
# 目录
SRC_DIR := src/bun.js/bindings
MODULES_DIR := src/bun.js/modules

# 调试对象文件目录
DEBUG_OBJ_DIR := src/bun.js/debug-bindings-obj

#源文件
SRC_FILES := $(wildcard $(SRC_DIR)/*.cpp)
MODULES_FILES := $(wildcard $(MODULES_DIR)/*.cpp)
SRC_WEBCORE_FILES := $(wildcard $(SRC_DIR)/webcore/*.cpp)
SRC_SQLITE_FILES := $(wildcard $(SRC_DIR)/sqlite/*.cpp)
SRC_NODE_OS_FILES := $(wildcard $(SRC_DIR)/node_os/*.cpp)
SRC_BUILTINS_FILES := $(wildcard src/js/out/*.cpp)
SRC_IO_FILES := $(wildcard src/io/*.cpp)
SRC_WEBCRYPTO_FILES := $(wildcard $(SRC_DIR)/webcrypto/*.cpp)

# 调试对象文件
DEBUG_OBJ_FILES := $(patsubst $(SRC_DIR)/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(SRC_FILES))
DEBUG_WEBCORE_OBJ_FILES := $(patsubst $(SRC_DIR)/webcore/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(SRC_WEBCORE_FILES))
DEBUG_SQLITE_OBJ_FILES := $(patsubst $(SRC_DIR)/sqlite/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(SRC_SQLITE_FILES))
DEBUG_NODE_OS_OBJ_FILES := $(patsubst $(SRC_DIR)/node_os/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(SRC_NODE_OS_FILES))
DEBUG_BUILTINS_OBJ_FILES := $(patsubst src/js/out/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(SRC_BUILTINS_FILES))
DEBUG_IO_FILES := $(patsubst src/io/%.cpp,$(DEBUG_OBJ_DIR)/%.o, $(SRC_IO_FILES))
DEBUG_MODULES_OBJ_FILES := $(patsubst ${MODULES_DIR}/%.cpp,$(DEBUG_OBJ_DIR)/%.o,$(MODULES_FILES))
DEBUG_WEBCRYPTO_OBJ_FILES := $(patsubst $(SRC_DIR)/webcrypto/%.cpp, $(DEBUG_OBJ_DIR)/%.o, $(SRC_WEBCRYPTO_FILES))



.PHONY: bindings
bindings: $(DEBUG_OBJ_DIR) \
  $(DEBUG_OBJ_FILES) \
  $(DEBUG_WEBCORE_OBJ_FILES) \
  $(DEBUG_SQLITE_OBJ_FILES) \
  $(DEBUG_NODE_OS_OBJ_FILES) \
  $(DEBUG_BUILTINS_OBJ_FILES) \
  $(DEBUG_IO_FILES) \
  $(DEBUG_MODULES_OBJ_FILES) \
  $(DEBUG_WEBCRYPTO_OBJ_FILES)
```