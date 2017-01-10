```java
public class ViewPagerAdapter extends FragmentPagerAdapter {
    private final List<Fragment> fragmentList = new ArrayList<>();
    private final List<String> fragmentTitleList = new ArrayList<>();

    public ViewPagerAdapter(FragmentManager manager) {
        super(manager);
    }

    @Override
    public Fragment getItem(int position) {
        return fragmentList.get(position);
    }

    @Override
    public int getCount() {
        return fragmentList.size();
    }

    public void addFrag(Fragment fragment, String title) {
        fragmentList.add(fragment);
        fragmentTitleList.add(title);
    }

    public void removeAllFragments(){
        fragmentList.clear();
        fragmentTitleList.clear();
    }

    @Override
    public CharSequence getPageTitle(int position) {
        return fragmentTitleList.get(position);
    }

}
```

