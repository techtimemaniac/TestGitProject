# TestGitProject
To Learn usage of GIT
public void assertElement(MobileElement element, String key, String value){

        Assert.assertNotNull("Element not registered, " + key, element);
        Assert.assertTrue("Element not found on screen, " + key, driver.isElementDisplay(element));

        if(StringUtils.isNotBlank(value)){
            Assert.assertEquals("Invalid copies, " + key, value, element.getText());
        }

    }

    public void assertElementNotPresent(MobileElement element, String key, String value){

        Assert.assertNotNull("Element not registered, " + key, element);
        Assert.assertFalse("Element not found on screen, " + key, driver.isElementDisplay(element));

    }

    public void assertMap(Map<String, String> expectedMap,          // From gherkin / feature file
                          Map<String, MobileElement> actualMap){    // From elementMap  (Name - Proxy Object map)

        for(Map.Entry<String, String> item : expectedMap.entrySet()){

            String key = item.getKey();
            String value = item.getValue();
            MobileElement mobileElement = actualMap.get(key);

            assertElement(mobileElement, key, value);
        }
    }

    public void assertMapNotPresent(Map<String, String> expectedMap,          // From gherkin / feature file
                                    Map<String, MobileElement> actualMap){
        for(Map.Entry<String, String> item : expectedMap.entrySet()){

            String key = item.getKey();
            String value = item.getValue();
            MobileElement mobileElement = actualMap.get(key);

            assertElementNotPresent(mobileElement, key, value);
        }
    }

    public void assertElementTextEquals(MobileElement element, String value){
        Assert.assertNotNull("No such element " + element ,element);
        String elementText = element.getText();
        Assert.assertEquals(value,elementText);
    }

    public void assertElementPresent(MobileElement element){
        Assert.assertTrue("No such element " + element , driver.isElementDisplay(element) || element.isEnabled());
    }

    public void assertElementNotPresent(MobileElement element){
        Assert.assertFalse("No such element " + element , driver.isElementDisplay(element) || element.isEnabled());
    }

    public void assertElementEnable(MobileElement element,Boolean status){
        if(status.equals(true)){
            Assert.assertTrue(element.isEnabled());
        } else {
            Assert.assertFalse(element.isEnabled());
        }
    }

    public void assertElementTextIsEmpty(MobileElement element){
        String elementText = element.getText();
        Assert.assertTrue(elementText.isEmpty());
    }
