# Android-Unit-Testing
This is a simple introduction to unit testing a Fragment in Android using Robolectric unit testing framework. 

## #Android #Robolectric #Mockito #PowerMocks

```
@RunWith(RobolectricGradleTestRunner.class)
@Config(constants = BuildConfig.class, sdk = 21, application = MyApplicationTest.class)
@PowerMockIgnore({"org.mockito.*", "org.robolectric.*", "android.*"})
@PrepareForTest({})
public class FragmentTest {

    private Fragment fragment;

    /**
     * Setup resources in here
     */
    @Before
    public void setUp() throws Exception {
        // Initialize mockito annotations (required only when to mock)
        MockitoAnnotations.initMocks(this);
        fragment = MyFragment.newInstance();
    }

    /**
     * Perform clean up after test
     */
    @After
    public void tearDown() throws Exception {
        fragment = null;
    }

    /**
    * Verify new instance creation of the fragment
    */
    @Test
    public void newInstance() throws Exception {
        Assert.assertNotNull(fragment);
    }

    /**
     * Verify event listeners/ callback bindings if any.
     */
    @Test
    public void testListeners() throws Exception {
      // 1) Verify that instance of the fragment is created
      Assert.assertNotNull(fragment);

      // 2) Start the fragment
      MyFragment myFragment = (MyFragment) fragment;
      SupportFragmentTestUtil.startFragment(fragment, AppCompatActivity.class);

      // 3) Verify all listeners and event interactions, associated to the view elements
      Assert.assertTrue(myFragment.getSomeUIView().hasOnClickListeners());
      Assert.assertNotNull(myFragment.getSomeUIView.getOnFocusChangeListener());
    }

    /**
     * Verify/ test that all validation criterias set on the form elements/ ui views 
	   * are working as expected. 
     */
    @Test
    public void testValidations() throws Exception {
      // 1) Start the fragment using SupportFragmentTestUtil of Roboelectric framework

      // 2) Assert that some UI view has no errors prior to validations
      Assert.assertNull(myFragment.getSomeUIView().getError());

      // 3) Invoke some method that will initiate the validations on some ui view

      // 4) Assert for error state of the ui view if validations should fail for some conditions. 
      Assert.assertNotNull(myFragment.getSomeUIView().getError());
    }

    /**
     * Verify that on invoking fragment onDetach life cycle method will perform necessary cleanup
     * so as to prevent memory leaks. Ensure that all views and resources are released when onDetach is invoked.
     */
    @Test
    public void testFragmentOnDetach() {
		    // 1) Start the fragment
        MyFragment myFragment = (MyFragment) fragment;
        SupportFragmentTestUtil.startFragment(fragment, AppCompatActivity.class);

        // 2) Perform fragment detach
        myFragment.onDetach();

        // 3) Verify views/ resources are released. Verify that some services are stopped. Verify that some event are un-registered
        Assert.assertNull(myFragment.somethingIsRelease());
    }

    /**
     * Verify UI views
     */
    @Test
    public void testViewsOfFragment() throws Exception {
        // 1) Verify that instance of the fragment is created
        MyFragment myFragment = (myFragment) fragment;

        // 2) Start the fragment
        SupportFragmentTestUtil.startFragment(fragment, AppCompatActivity.class);

        // 3) Verify for any of the UI properties

        Assert.assertTrue(myFragment.getSomeUIView().isEnabled());
        Assert.assertTrue(myFragment.getSomeUIView().getVisibility() == View.INVISIBLE);
    }
	
    /**
    * Verify any data adapter in the fragment
    */
    @Test
    public void testSomeAdapter() throws Exception {

      // 1) Start the fragment

      // 2) Verify that data adapter is not null. This is the adapter the will be loaded into the ViewPager
      Assert.assertNotNull(myFragment.getMyAdapter());

      // 3) Verify that the Adapter contains some data elements.
      Assert.assertTrue(myFragment.getMyAdapter().getCount() == 2);

      // 4) Assert that adapter is the instance of MyViewPagerAdapter
      Assert.assertTrue(myFragment.getViewPager().getAdapter() instanceof MyFragment.MyViewPagerAdapter);

      MyFragment.MyViewPagerAdapter viewPagerAdapter
          = (MyFragment.MyViewPagerAdapter) myFragment.getViewPager().getAdapter();

      // 5) Assert that the viewpager contains some elements
      Assert.assertTrue(viewPagerAdapter.getCount() == 2);

      // 6) Assert that the viewpager adapter method getItem returns a valid item
      Assert.assertNotNull(viewPagerAdapter.getItem(0));
      Assert.assertNotNull(viewPagerAdapter.getItem(1));

      // 7) Assert that viewpager changes page dynamically on any action
      myFragment.onButtonClick();
      Assert.assertTrue(myFragment.getViewPager().getCurrentItem() == 1);
    }

}
```
