# Shared Buffer

In several scenarios we are interested in using shared buffers made of a pointer and a size. The pointer can be originated from various
entities such as a shared_ptr of a vector, string, or constant memory pointers.

We can use the deleter property of such shared pointer for controlling the references. Below we provides some examples

  struct buffer
  {
    const uint8_t * data;
    int size;
  };
  
  std::shared_ptr<buffer> makebuffer(const char * p, int size)  
  {
    return std::shared_ptr<buffer>(new buffer(p,sie),[] (buffer *p) {});
  }
  
  std::shared_ptr<buffer> makebuffer(std::shared_ptr<std::vector<uint8_t> q)  
  {
    return std::shared_ptr<buffer>(new buffer(q->data(),q->size()),[q] (buffer *p) mutable {});
  }
  
  std::shared_ptr<buffer> makebuffer(std::shared_ptr<std::string> q)  
  {
    return std::shared_ptr<buffer>(new buffer((uint8_t*)q->c_str(),q->size()),[q] (buffer *p) mutable {});
  }
  
  std::shared_ptr<buffer> makebuffer(std::shared_ptr<buffer> q, int offset, int size)  
  {    
    return std::shared_ptr<buffer>(new buffer(q->data+offet,size),[q] (buffer *p) mutable {});
  }  
