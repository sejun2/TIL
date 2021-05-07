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
NOTE: Here I have used the backing property and assign a binding variable to null in the onDestroyeView() method because Fragments outlive their views. 
Make sure you clean up any references to the binding class instance in the fragment’s onDestroyView() method.
