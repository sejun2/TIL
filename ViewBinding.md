```kotlin
class SampleFragment : Fragment() {

    private var _binding: FragmentSampleBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Inflate the layout for this fragment
        _binding = FragmentSampleBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }

}
```
프래그먼트에서 뷰 바인딩을 활용할경우 onDestroyView에서 Binding 인스턴스를 해제 해주어야한다.   
NOTE: Here I have used the backing property and assign a binding variable to null in the onDestroyeView() method because Fragments outlive their views. 
Make sure you clean up any references to the binding class instance in the fragment’s onDestroyView() method.

Advantages of View Binding:     
Null safety:    
Because View Binding directly creates a reference to views, there is not any risk of a null pointer exception.  
Type safety:    
The Generated binding class has the same class types fields as present in the XML file, so there are no risks for class cast exceptions.    

