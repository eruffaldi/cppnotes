

Metaprogramming Snippets

# Restrict template type as derived from class

    #include <type_traits>

    template<typename D, typename B>
    using extends = typename std::enable_if<std::is_base_of<B,D>::value>::type;

    template <class T, typename X = extends<MYCLASS,T> >
    void MYFX(T & p);

    
