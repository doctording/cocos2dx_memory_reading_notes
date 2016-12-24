内存相关知识
===
# 内存概念
内存是计算机中重要的部件之一，它是与CPU进行沟通的桥梁。计算机中所有程序的运行都是在内存中进行的，因此内存的性能对计算机的影响非常大。内存(Memory)也被称为内存储器，其作用是用于暂时存放CPU中的运算数据，以及与硬盘等外部存储器交换的数据。只要计算机在运行中，CPU就会把需要运算的数据调到内存中进行运算，当运算完成后CPU再将结果传送出来，内存的运行也决定了计算机的稳定运行。 内存是由内存芯片、电路板、金手指等部分组成的。

# 程序的执行过程

编译，链接，装入

* 静态链接
各目标模块链接成为一个可执行程序，不在拆分

* 装入时动态链接
将用户源程序编译后所得到的一组目标模块，在装入内存时，采用边转入边连接的方式

* 运行时动态链接
在程序执行过程中，需要改模块时对它进行链接。
其优点是 便于修改和更新，实现对目标模块的共享

# 多道程序环境下的扩充内存的方法

* 覆盖

打破了一个进程的全部信息必须一次全部装入主存后才能运行的限制，内存的可覆盖位置可以及时的更新

* 交换

把处于等待状态的程序的内存移动到辅存中，空间腾出来让准备好竞争CPU运行的程序从辅存中移动到内存中

交换技术主要是在不同进程之间进行, 覆盖主要用于同一程序或进程中

## 连续分配管理方式

* 单一连续分配
* 固定分区分配
* 动态分区分配
	* 首次适应
	* 最佳适应
	* 临近适应

## 非连续分配管理方式

**将程序分散的转入到不相邻的内存分区中**。在连续分配管理方式中，及时有足够的内存空间，但是如果不连续，程序也是无法运行的。

* 基本分页

不会产生外部碎片，会有页内碎片。

* 基本分段

* 段页

#虚拟内存

## 原理的内存管理的特点和不足之处

上述的内存管理策略都是为了**同时将多个进程保存到内存中**以便允许多道程序能够运行，具有如下的特征

* 一次性： 作业必须一次性全部装入内存后才能够开始运行
	大作业可能无法运行
	作业很多时，只能少数的作业先运行，导致多道程序度的下降

* 驻留性
	作业装入内存后，一直驻留在内存中，其任何部分都不换出，直到运行结束。运行中的进程，会因为等待I/O而等待，可能长期处于等待状态

## 局部性原理
	
* 时间局部性
	
* 空间局部性	

## 虚拟内存引入

**局部性原理**，程序在装入时，可以将程序的一部分装入内存，而将其余部分留在外存中
启动程序后，在程序执行过程中，当所访问的信息不在内存时，有操作系统将所需要的部分调用内存中，然后继续程序的执行。另一方面，操作系统将内存中暂时不适用的内存换出到外存中。这样系统提供的内存好像比实际内存要大一些，---虚拟存储

虚拟存储的三个特征

* 多次性
	作业不需要一次性的全部装入内存，而是可以被分成多次调入内存

* 对换性
	作业运行时，不需要一直常驻内存，而是可以允许作业在运行过程中，进行换入换出

* 虚拟性
	从逻辑上扩容，实际上内存差不多（或者更小了）

## 虚拟内存的实现

* 请求分页, 请求分段, 请求段页
* 一定量的内存和外存
* 页表机制
* 中断机制
* 地址变换机制
* 页面置换算法
    * 最佳置换 OPT
    * 先进先出页面置换算法 FIFO(Belady现象)
    * 最近最久未使用置换算法 （LRU）
    * 时钟置换算法（CLOCK）

# C++中常见的内存问题

* 野指针

一些内存已经被释放了，之前指向它的指针仍然在被利用。这些内存有可能在运行时被系统重新分配给其他程序使用，从而导致无法预测的错误

* 重复释放

释放已经释放了的内存，或者释放配重新分配过的内存
delete/free后，应该接着设置为NULL;

* 内存泄漏

不再使用的内存单元如果没有被释放就会导致内存泄漏。如果程序不断地重复进行这类操作，将会导致内存占用剧增

# 内存碎片
内存碎片：**描述一个系统中所有的不可用的空闲内存**；这些资源之所以仍然未被使用，是因为负责分配内存的分配器使这些内存无法使用。这一问题通常都会发生，原因在于空闲内存以**小而不连续方式**出现在不同的位置。由于分配方法决定内存碎片是否是一个问题，因此内存分配器在保证空闲资源可用性方面扮演着重要的角色。

## 内存碎片产生的原因
原因在与空闲内存以小而不连续的方式出现在不同的位置(内存分配较小，并且分配的这些小的内存生存周期又较长，反复申请后将产生内存碎片的出现)。内存分配程序浪费内存的基本方式有三种：即额外开销、内部碎片以及外部碎片

* 内存分配程序需要遵循一些基本的内存分配规则。例如，所有的内存分配必须起始于可被 4、8 或 16 整除（视处理器体系结构而定）的地址。内存分配程序把仅仅预定大小的内存块分配给客户，可能还有其它原因。当某个客户请求一个 43 字节的内存块时，它可能会获得 44字节、48字节 甚至更多的字节。由所需大小四舍五入而产生的多余空间就叫**内部碎片**。

* 外部碎片的产生是当已分配内存块之间出现未被使用的差额时，就会产生外部碎片。例如，一个应用程序分配三个连续的内存块，然后使中间的一个内存块空闲。内存分配程序可以重新使用中间内存块供将来进行分配，但不太可能分配的块正好与全部空闲内存一样大。倘若在运行期间，内存分配程序不改变其实现法与四舍五入策略，则额外开销和内部碎片在整个系统寿命期间保持不变。虽然额外开销和内部碎片会浪费内存，因此是不可取的，但外部碎片才是嵌入系统开发人员真正的敌人，造成系统失效的正是分配问题。

## 如何避免内存碎片的产生
1 少用动态内存分配的函数(尽量使用栈空间)
2 分配内存和释放的内存尽量在同一个函数中
3 尽量一次性申请较大的内存2的指数次幂大小的内存空间，而不要反复申请小内存(少进行内存的分割)
4 使用内存池来减少使用堆内存引起的内存碎片
5 尽可能少地申请空间。
6 尽量少使用堆上的内存空间~
7 做**内存池**，也就是自己一次申请一块足够大的空间，然后自己来管理，用于大量频繁地new/delete操作。

# 内存垃圾回收的几种机制
## 1.引用计数算法

引用计数（Reference Counting）算法是每个对象计算指向它的指针的数量，当有一个指针指向自己时计数值加1；当删除一个指向自己的指针时，计数值减1，如果计数值减为0，说明已经不存在指向该对象的指针了，所以它可以被安全的销毁了。


## 引用计数的明显缺点：无法处理环形引用

遍历所有的栈去解决

### 算法特点

1. 需要单独的字段存储计数器，增加了存储空间的开销；

2. 每次赋值都需要更新计数器，增加了时间开销；

3. 垃圾对象便于辨识，只要计数器为0，就可作为垃圾回收；

4. 及时回收垃圾，没有延迟性；

5. 不能解决循环引用的问题；

## 2. 标记-清除(Mark-Sweep)算法

例如: Lua就采用了mark-sweep的垃圾回收机制

标记-清除（Mark-Sweep）算法依赖于对所有存活对象进行一次全局遍历来确定哪些对象可以回收，遍历的过程从根出发，找到所有可达对象，除此之外，其它不可达的对象就是垃圾对象，可被回收。整个过程分为两个阶段：标记阶段找到所有存活对象；清除阶段清除所有垃圾对象。


![](http://i.imgur.com/3fORCpQ.png)

![](http://i.imgur.com/MgO8moL.png)

### 优点

* 相比较引用计数算法，标记-清除算法可以非常自然的处理环形引用问题，

* 另外在创建对象和销毁对象时时少了操作引用计数值的开销


### 缺点

* 标记-清除算法是一种“停止-启动”算法，在垃圾回收器运行过程中，应用程序必须暂时停止

* 标记-清除算法在标记阶段需要遍历所有的存活对象，会造成一定的开销

* 在清除阶段，清除垃圾对象后会造成大量的内存碎片。


C++中 shared_ptr就使用了引用计数（reference counting）。

## 3. 标记-缩并算法(解决内存碎片)

整个过程可以描述为
* 标记所有的存活对象；
* 通过重新调整存活对象位置来缩并对象图；
* 更新指向被移动了位置的对象的指针。

标记-压缩算法最大的难点在于如何选择所使用的压缩算法，如果压缩算法选择不好，将会导致极大的程序性能问题，如导致Cache命中率低等。一般来说，根据压缩后对象的位置不同，压缩算法可以分为以下三种：

1. 任意：移动对象时不考虑它们原来的次序，也不考虑它们之间是否有互相引用的关系。 

2. 线性：尽可能的将原来的对象和它所指向的对象放在相邻位置上，这样可以达到更好的空间局部性。 

3. 滑动：将对象“滑动”到堆的一端，把存活对象之间的自由单元“挤出去”，从而维持了分配时的原始次序。


## 4. 节点拷贝算法（解决内部碎片）

节点拷贝算法是把整个堆分成两个半区（From，To）， GC的过程其实就是把存活对象从一个半区From拷贝到另外一个半区To的过程，而在下一次回收时，两个半区再互换角色。在移动结束后，再更新对象的指针引用

参考
http://blog.csdn.net/sinat_36246371/article/details/53002209
![](http://i.imgur.com/rriCibI.png)

简单，高效，空间换时间。


cocos2dx内存管理(cocos2dx 3.0)
===
cocos2d-x中使用**引用计数来管理内存**，增加了**自动回收池**。

# Ref对象
cocos2d-x中通过Ref类（在2.x中是CCObject）来实现引用计数，所有需要实现内存自动回收的类都应该继承自Ref类。对象创建时引用计数为1，对象拷贝时引用计数加1，对象释放时引用计数减1，如果引用计数为0时释放对象内存。

```
Ref::Ref()
: _referenceCount(1) // when the Ref is created, the reference count of it is 1
{
#if CC_ENABLE_SCRIPT_BINDING
    static unsigned int uObjectCount = 0;
    _luaID = 0;
    _ID = ++uObjectCount;
#endif
}

Ref::~Ref()
{
#if CC_ENABLE_SCRIPT_BINDING
    // if the object is referenced by Lua engine, remove it
    if (_luaID)
    {
        ScriptEngineManager::getInstance()->getScriptEngine()->removeScriptObjectByObject(this);
    }
    else
    {
        ScriptEngineProtocol* pEngine = ScriptEngineManager::getInstance()->getScriptEngine();
        if (pEngine != NULL && pEngine->getScriptType() == kScriptTypeJavascript)
        {
            pEngine->removeScriptObjectByObject(this);
        }
    }
#endif
}

void Ref::retain()
{
    CCASSERT(_referenceCount > 0, "reference count should greater than 0");
    ++_referenceCount;
}

void Ref::release()
{
    CCASSERT(_referenceCount > 0, "reference count should greater than 0");
    --_referenceCount;
    
    if (_referenceCount == 0)
    {
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
        auto poolManager = PoolManager::getInstance();
        if (!poolManager->getCurrentPool()->isClearing() && poolManager->isObjectInPools(this))
        {
          
        }
#endif
        delete this;
    }
}

Ref* Ref::autorelease()
{
    PoolManager::getInstance()->getCurrentPool()->addObject(this);
    return this;
}

unsigned int Ref::getReferenceCount() const
{
    return _referenceCount;
}

```

# PoolManager类的定义
```
class CC_DLL PoolManager
{
public:
    /**
     * @js NA
     * @lua NA
     */
    CC_DEPRECATED_ATTRIBUTE static PoolManager* sharedPoolManager() { return getInstance(); }
    static PoolManager* getInstance();
    
    /**
     * @js NA
     * @lua NA
     */
    CC_DEPRECATED_ATTRIBUTE static void purgePoolManager() { destroyInstance(); }
    static void destroyInstance();
    
    /**
     * Get current auto release pool, there is at least one auto release pool that created by engine.
     * You can create your own auto release pool at demand, which will be put into auto releae pool stack.
     */
    AutoreleasePool *getCurrentPool() const;

    bool isObjectInPools(Ref* obj) const;

    /**
     * @js NA
     * @lua NA
     */
    friend class AutoreleasePool;
    
private:
    PoolManager();
    ~PoolManager();
    
    void push(AutoreleasePool *pool);
    void pop();
    
    static PoolManager* s_singleInstance;
    
    std::deque<AutoreleasePool*> _releasePoolStack;
    AutoreleasePool *_curReleasePool;
};
```

## PoolManager类的一些成员方法
````
PoolManager* PoolManager::s_singleInstance = nullptr;

PoolManager* PoolManager::getInstance()
{
    if (s_singleInstance == nullptr)
    {
        s_singleInstance = new PoolManager();
        // Add the first auto release pool
        s_singleInstance->_curReleasePool = new AutoreleasePool("cocos2d autorelease pool");
        s_singleInstance->_releasePoolStack.push_back(s_singleInstance->_curReleasePool);
    } 
    return s_singleInstance;
}

AutoreleasePool::AutoreleasePool()
: _name("")
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
, _isClearing(false)
#endif
{
    _managedObjectArray.reserve(150);
    PoolManager::getInstance()->push(this);
}

AutoreleasePool::AutoreleasePool(const std::string &name)
: _name(name)
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
, _isClearing(false)
#endif
{
    _managedObjectArray.reserve(150);
    PoolManager::getInstance()->push(this);
}

```

## AutoreleasePool类的定义
```
class CC_DLL AutoreleasePool
{
public:
    /**
     * @warn Don't create an auto release pool in heap, create it in stack.
     * @js NA
     * @lua NA
     */
    AutoreleasePool();
    
    /**
     * Create an autorelease pool with specific name. This name is useful for debugging.
     */
    AutoreleasePool(const std::string &name);
    
    /**
     * @js NA
     * @lua NA
     */
    ~AutoreleasePool();

    /**
     * Add a given object to this pool.
     *
     * The same object may be added several times to the same pool; When the
     * pool is destructed, the object's Ref::release() method will be called
     * for each time it was added.
     *
     * @param object    The object to add to the pool.
     * @js NA
     * @lua NA
     */
    void addObject(Ref *object);

    /**
     * Clear the autorelease pool.
     *
     * Ref::release() will be called for each time the managed object is
     * added to the pool.
     * @js NA
     * @lua NA
     */
    void clear();
    
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
    /**
     * Whether the pool is doing `clear` operation.
     */
    bool isClearing() const { return _isClearing; };
#endif
    
    /**
     * Checks whether the pool contains the specified object.
     */
    bool contains(Ref* object) const;

    /**
     * Dump the objects that are put into autorelease pool. It is used for debugging.
     *
     * The result will look like:
     * Object pointer address     object id     reference count
     *
     */
    void dump();
    
private:
    /**
     * The underlying array of object managed by the pool.
     *
     * Although Array retains the object once when an object is added, proper
     * Ref::release() is called outside the array to make sure that the pool
     * does not affect the managed object's reference count. So an object can
     * be destructed properly by calling Ref::release() even if the object
     * is in the pool.
     */
    std::vector<Ref*> _managedObjectArray;
    std::string _name;
    
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
    /**
     *  The flag for checking whether the pool is doing `clear` operation.
     */
    bool _isClearing;
#endif
};
```

# cocos2dx程序运行

cocos2dx从main函数运行，创建 AppDelegate单例对象，并运行run方法，

```
int Application::run()
{
    PVRFrameEnableControlWindow(false);

    // Main message loop:
    LARGE_INTEGER nFreq;
    LARGE_INTEGER nLast;
    LARGE_INTEGER nNow;

    QueryPerformanceFrequency(&nFreq);
    QueryPerformanceCounter(&nLast);

    // Initialize instance and cocos2d.
    if (!applicationDidFinishLaunching())
    {
        return 0;
    }

    auto director = Director::getInstance();
    auto glview = director->getOpenGLView();

    // Retain glview to avoid glview being released in the while loop
    glview->retain();

    while(!glview->windowShouldClose())
    {
        QueryPerformanceCounter(&nNow);
        if (nNow.QuadPart - nLast.QuadPart > _animationInterval.QuadPart)
        {
            nLast.QuadPart = nNow.QuadPart;
            
            director->mainLoop();
            glview->pollEvents();
        }
        else
        {
            Sleep(0);
        }
    }

    // Director should still do a cleanup if the window was closed manually.
    if (glview->isOpenGLReady())
    {
        director->end();
        director->mainLoop();
        director = nullptr;
    }
    glview->release();
    return true;
}
```

AppDelegate对象运行的mainLoop()方法，每一帧循环都调用画图，内存池释放内存的方法（当对象的引用计数为0时，若没有调用retain方法，就会被delete）
```
void DisplayLinkDirector::mainLoop()
{
    if (_purgeDirectorInNextLoop)
    {
        _purgeDirectorInNextLoop = false;
        purgeDirector();
    }
    else if (! _invalid)
    {
        drawScene();
     
        // release the objects
        PoolManager::getInstance()->getCurrentPool()->clear();
    }
}
```

```
void AutoreleasePool::clear()
{
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
    _isClearing = true;
#endif
    for (const auto &obj : _managedObjectArray)
    {
        obj->release();
    }
    _managedObjectArray.clear();
#if defined(COCOS2D_DEBUG) && (COCOS2D_DEBUG > 0)
    _isClearing = false;
#endif
}
```

cocos2dx中的几个重要对象和缓存对象
===

# cocos2dx Node类

Node对象是最基础的场景图像对象，场景类，图层类，精灵类，菜单类等都需要继承这个类，Node类（继承了Ref类）的说明如下

```
/** @brief Node is the base element of the Scene Graph. Elements of the Scene Graph must be Node objects or subclasses of it.
 The most common Node objects are: Scene, Layer, Sprite, Menu, Label.

 The main features of a Node are:
 - They can contain other Node objects (`addChild`, `getChildByTag`, `removeChild`, etc)
 - They can schedule periodic callback (`schedule`, `unschedule`, etc)
 - They can execute actions (`runAction`, `stopAction`, etc)

 Subclassing a Node usually means (one/all) of:
 - overriding init to initialize resources and schedule callbacks
 - create callbacks to handle the advancement of time
 - overriding `draw` to render the node

 Properties of Node:
 - position (default: x=0, y=0)
 - scale (default: x=1, y=1)
 - rotation (in degrees, clockwise) (default: 0)
 - anchor point (default: x=0, y=0)
 - contentSize (default: width=0, height=0)
 - visible (default: true)

 Limitations:
 - A Node is a "void" object. If you want to draw something on the screen, you should use a Sprite instead. Or subclass Node and override `draw`.

 */

class CC_DLL Node : public Ref
```

## Sprite精灵类的说明如下，Sprite类集成了Node类和TextureProtocol类
```
/**
 * Sprite is a 2d image ( http://en.wikipedia.org/wiki/Sprite_(computer_graphics) )
 *
 * Sprite can be created with an image, or with a sub-rectangle of an image.
 *
 * To optimize the Sprite rendering, please follow the following best practices:
 *
 *  - Put all your sprites in the same spritesheet (http://www.codeandweb.com/what-is-a-sprite-sheet)
 *  - Use the same blending function for all your sprites
 *  - ...and the Renderer will automatically "batch" your sprites (will draw all of them in one OpenGL call).
 *
 *  To gain an additional 5% ~ 10% more in the rendering, you can parent your sprites into a `SpriteBatchNode`.
 *  But doing so carries the following limitations:
 *
 *  - The Alias/Antialias property belongs to `SpriteBatchNode`, so you can't individually set the aliased property.
 *  - The Blending function property belongs to `SpriteBatchNode`, so you can't individually set the blending function property.
 *  - `ParallaxNode` is not supported, but can be simulated with a "proxy" sprite.
 *  - Sprites can only have other Sprites (or subclasses of Sprite) as children.
 *
 * The default anchorPoint in Sprite is (0.5, 0.5).
 */
class CC_DLL Sprite : public Node, public TextureProtocol
```

Sprite 根据一个图片路径创建时，纹理部分调用了TextureProtocol类的相关方法，这里使用了纹理缓存,下面是先关的Sprite创建过程的代码

```
// add "HelloWorld" splash screen"
auto sprite = Sprite::create("HelloWorld.png");

// position the sprite on the center of the screen
sprite->setPosition(Point(visibleSize.width/2 + origin.x, visibleSize.height/2 + origin.y));

// add the sprite as a child to this layer
this->addChild(sprite, 0);
```

```
Sprite::Sprite(void)
: _shouldBeHidden(false)
, _texture(nullptr)
, _insideBounds(true)
{
}

Sprite::~Sprite(void)
{
    CC_SAFE_RELEASE(_texture);
}

Sprite* Sprite::create(const std::string& filename)
{
    Sprite *sprite = new Sprite();
    if (sprite && sprite->initWithFile(filename))
    {
        sprite->autorelease();
        return sprite;
    }
    CC_SAFE_DELETE(sprite);
    return nullptr;
}

bool Sprite::initWithFile(const std::string& filename)
{
    CCASSERT(filename.size()>0, "Invalid filename for sprite");

    Texture2D *texture = Director::getInstance()->getTextureCache()->addImage(filename);
    if (texture)
    {
        Rect rect = Rect::ZERO;
        rect.size = texture->getContentSize();
        return initWithTexture(texture, rect);
    }

    // don't release here.
    // when load texture failed, it's better to get a "transparent" sprite then a crashed program
    // this->release();
    return false;
}
```

```
TextureCache* Director::getTextureCache() const
{
    return _textureCache;
}

void Director::initTextureCache()
{
#ifdef EMSCRIPTEN
    _textureCache = new TextureCacheEmscripten();
#else
    _textureCache = new TextureCache();
#endif // EMSCRIPTEN
}
```

## TextureCache纹理缓存类也是继承Ref类，说明如下
```
/**
 * @addtogroup textures
 * @{
 */
/*
* from version 3.0, TextureCache will never to treated as a singleton, it will be owned by director.
* all call by TextureCache::getInstance() should be replaced by Director::getInstance()->getTextureCache()
*/

/** @brief Singleton that handles the loading of textures
* Once the texture is loaded, the next time it will return
* a reference of the previously loaded texture reducing GPU & CPU memory
*/
class CC_DLL TextureCache : public Ref
```

由一个资源路径，TextureCache::addImage(const std::string &path)会先查找这个资源是否已经被加载了的，通过其成员变量std::unordered_map<std::string, Texture2D*> _textures;（一个map类型的资源路径到纹理的映射）

```
Texture2D * TextureCache::addImage(const std::string &path)
{
    Texture2D * texture = nullptr;
    Image* image = nullptr;
    // Split up directory and filename
    // MUTEX:
    // Needed since addImageAsync calls this method from a different thread

    std::string fullpath = FileUtils::getInstance()->fullPathForFilename(path);
    if (fullpath.size() == 0)
    {
        return nullp
    }
    auto it = _textures.find(fullpath);
    if( it != _textures.end() )
        texture = it->second;

    if (! texture)
    {
        // all images are handled by UIImage except PVR extension that is handled by our own handler
        do 
        {
            image = new Image();
            CC_BREAK_IF(nullptr == image);

            bool bRet = image->initWithImageFile(fullpath);
            CC_BREAK_IF(!bRet);

            texture = new Texture2D();

            if( texture && texture->initWithImage(image) )
            {
#if CC_ENABLE_CACHE_TEXTURE_DATA
                // cache the texture file name
                VolatileTextureMgr::addImageTexture(texture, fullpath);
#endif
                // texture already retained, no need to re-retain it
                _textures.insert( std::make_pair(fullpath, texture) );
            }
            else
            {
                CCLOG("cocos2d: Couldn't create texture for file:%s in TextureCache", path.c_str());
            }
        } while (0);
    }

    CC_SAFE_RELEASE(image);

    return texture;
}
```

## Textures 有如下的一些清除纹理方法
```
/** Purges the dictionary of loaded textures.
* Call this method if you receive the "Memory Warning"
* In the short term: it will free some resources preventing your app from being killed
* In the medium term: it will allocate more resources
* In the long term: it will be the same
*/
void removeAllTextures();

/** Removes unused textures
* Textures that have a retain count of 1 will be deleted
* It is convenient to call this method after when starting a new Scene
* @since v0.8
*/
void removeUnusedTextures();

/** Deletes a texture from the cache given a texture
*/
void removeTexture(Texture2D* texture);

/** Deletes a texture from the cache given a its key name
@since v0.99.4
*/
void removeTextureForKey(const std::string &key);


void TextureCache::removeAllTextures()
{
    for( auto it=_textures.begin(); it!=_textures.end(); ++it ) {
        (it->second)->release();
    }
    _textures.clear();
}

void TextureCache::removeUnusedTextures()
{
    for( auto it=_textures.cbegin(); it!=_textures.cend(); /* nothing */) {
        Texture2D *tex = it->second;
        if( tex->getReferenceCount() == 1 ) {
            CCLOG("cocos2d: TextureCache: removing unused texture: %s", it->first.c_str());

            tex->release();
            _textures.erase(it++);
        } else {
            ++it;
        }

    }
}

void TextureCache::removeTexture(Texture2D* texture)
{
    if( ! texture )
    {
        return;
    }

    for( auto it=_textures.cbegin(); it!=_textures.cend(); /* nothing */ ) {
        if( it->second == texture ) {
            texture->release();
            _textures.erase(it++);
            break;
        } else
            ++it;
    }
}

void TextureCache::removeTextureForKey(const std::string &textureKeyName)
{
    std::string key = textureKeyName;
    auto it = _textures.find(key);

    if( it == _textures.end() ) {
        key = FileUtils::getInstance()->fullPathForFilename(textureKeyName);
        it = _textures.find(key);
    }

    if( it != _textures.end() ) {
        (it->second)->release();
        _textures.erase(it);
    }
}
```

## TextureCache总结
TextureCache缓存加载纹理资源到内存中，也就是图片资源。其原理是对加入缓存的纹理资源进行一次引用，使其引用计数加一，保持不被清除，而Cocos2d-x 的渲染机制是可以重复使用同一份纹理在不同的场合进行绘制，从而达到重复使用，降低内存和GPU 运算资源的开销的目的。
一般情况下，我们应该在切换场景时清理缓存中的无用纹理，因为不同场景间使用的纹理是不同的。如果确实存在着共享的纹理，将其加入一个标记数组来保持其引用计数，以避免被清理了。 


# SpriteFrameCache
SpriteFrameCache 主要服务于多张碎图合并出来的纹理图片。这种纹理在一张大图中包含了多张小图，直接通过TextureCache引用会有诸多不便，因而衍生出来精灵框帧的处理方式，即把截取好的纹理信息保存在一个精灵框帧内，精灵通过切换不同的框帧来显示出不同的图案。

```
class Sprite;

class CC_DLL SpriteFrameCache : public Ref
{
public:
 
    static SpriteFrameCache* getInstance(void);

    CC_DEPRECATED_ATTRIBUTE static SpriteFrameCache* sharedSpriteFrameCache() { return SpriteFrameCache::getInstance(); }

    static void destroyInstance();

    CC_DEPRECATED_ATTRIBUTE static void purgeSharedSpriteFrameCache() { return SpriteFrameCache::destroyInstance(); }

protected:
    SpriteFrameCache(){}

public:
    virtual ~SpriteFrameCache();
    bool init(void);

public:
  
    void addSpriteFramesWithFile(const std::string& plist);

    void addSpriteFramesWithFile(const std::string& plist, const std::string& textureFileName);

    void addSpriteFramesWithFile(const std::string&plist, Texture2D *texture);

    void addSpriteFrame(SpriteFrame *frame, const std::string& frameName);

    void removeSpriteFrames();

    void removeUnusedSpriteFrames();

    void removeSpriteFrameByName(const std::string& name);

    void removeSpriteFramesFromFile(const std::string& plist);

    void removeSpriteFramesFromTexture(Texture2D* texture);

    SpriteFrame* getSpriteFrameByName(const std::string& name);

    void addSpriteFramesWithDictionary(ValueMap& dictionary, Texture2D *texture);

    void removeSpriteFramesFromDictionary(ValueMap& dictionary);

protected:
    Map<std::string, SpriteFrame*> _spriteFrames;
    ValueMap _spriteFramesAliases;
    std::set<std::string>*  _loadedFileNames;
};

```

SpriteFrameCache的内部封装了一个Map<std::string, SpriteFrame*> _spriteFrames对象。key为帧的名称。SpriteFrameCache一般用来处理plist文件(这个文件指定了每个独立的精灵在这张“大图”里面的位置和大小)，该文件对应一张包含多个精灵的大图，plist文件可以使用TexturePacker制作。

SpriteFrameCache的常用接口和TextureCache类似，不再赘述了，唯一需要注意的是添加精灵帧的配套文件一个plist文件和一张大的纹理图。


## sprite-sheet 纹理优化
Sprite表是一个有效的内存优化方法
参考如下
http://blog.csdn.net/qq_26437925/article/details/53857120

SpriteFrameCache跟TextureCache功能一样，将SpriteFrame缓存起来，在下次使用的时候直接去取。不过跟TextureCache不同的是，如果内存池中不存在要查找的图片，它会提示找不到，而不会去本地加载图片。

# AnimationCache 动画缓存
通常情况下，对于一个精灵动画，每次创建时都需要加载精灵帧，然后按顺序添加到数组，再用Animation读取数组创建动画。这是一个非常烦琐的计算过程。而对于使用频率高的动画，例如角色的走动、跳舞等，可以将其加入到AnimationCache中，每次使用都从这个缓存中调用，这样可以有效的降低创建动画的巨大消耗。

所以将创建好的动画Animation直接放在动画缓存AnimationCache中，当需要执行动画动作时，就直接从动画缓存中拿出来，去创建初始化Animation会非常方便。

```
class Animation;

/** Singleton that manages the Animations.
It saves in a cache the animations. You should use this class if you want to save your animations in a cache.

Before v0.99.5, the recommend way was to save them on the Sprite. Since v0.99.5, you should use this class instead.

@since v0.99.5
*/
class CC_DLL AnimationCache : public Ref
{
public:

    AnimationCache();

    ~AnimationCache();
    
    static AnimationCache* getInstance();

    static void destroyInstance();

    CC_DEPRECATED_ATTRIBUTE static AnimationCache* sharedAnimationCache() { return AnimationCache::getInstance(); }

    CC_DEPRECATED_ATTRIBUTE static void purgeSharedAnimationCache() { return AnimationCache::destroyInstance(); }

    bool init(void);

    void addAnimation(Animation *animation, const std::string& name);

     
    CC_DEPRECATED_ATTRIBUTE void removeAnimationByName(const std::string& name){ removeAnimation(name);}

    CC_DEPRECATED_ATTRIBUTE Animation* animationByName(const std::string& name){ return getAnimation(name); }

    void addAnimationsWithDictionary(const ValueMap& dictionary,const std::string& plist);

    void addAnimationsWithFile(const std::string& plist);

private:
    void parseVersion1(const ValueMap& animations);
    void parseVersion2(const ValueMap& animations);

private:
    Map<std::string, Animation*> _animations;
    static AnimationCache* s_sharedAnimationCache;
};
```

cocos2dx 如何优化内存使用
===
官方参考地址
http://www.cocos.com/docs/native/v2/optimizations/how-to-optimise-memory-usage/zh.html

## 推荐的内存优化技巧
* 一帧一帧载入游戏资源
* 减少绘制调用，使用“CCSpriteBatchNode”
* 载入纹理时按照从大到小的顺序
* 避免高峰内存使用
* 使用载入屏幕预载入游戏资源
* 需要时释放空闲资源
* 收到内存警告后释放缓存资源.
* 使用纹理打包器优化纹理大小、格式、颜色深度等
* 使用JPG格式要谨慎！
* 请使用RGB4444颜色深度16位纹理
* 请使用NPOT纹理，不要使用POT纹理
* 避免载入超大纹理
* 推荐1024*1024 NPOT pvr.ccz纹理集，而不要采用RAW PNG纹理

cocos2dx 内存优化，参考博文
http://xiaominghimi.blog.51cto.com/2614927/1085955/

# ...
