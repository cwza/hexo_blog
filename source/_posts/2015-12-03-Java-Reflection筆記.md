title: Java Reflection筆記
categories: [computer science, java]
tags: [java]
date: 2015-12-03 15:34:08
---
just some code note
<!-- more -->
``` java
public class ReflectionUtils {
	public Object newObjectByName(String className) throws Exception {
		return Class.forName(className).newInstance();
	}
	
	public Class<?> getMemberListType(String listName, Class<?> from) throws Exception {
		//Field listField = from.getDeclaredField(listName);
		Field listField = getDeclaredFieldRecursive(listName, from);
        ParameterizedType listType = (ParameterizedType) listField.getGenericType();
        Class<?> listClass = (Class<?>) listType.getActualTypeArguments()[0];
        return listClass;
	}
	
	public Field getDeclaredFieldRecursive(String fieldName, Class<?> from) {
		Field field = null;
		try {
			field = from.getDeclaredField(fieldName);
		} catch (NoSuchFieldException e) {
			field = getDeclaredFieldRecursive(fieldName,from.getSuperclass());
		} 
		return field;
	}
	
	public Class<?> getMemberType(String memberName, Class<?> from) throws Exception {
		Field field = from.getDeclaredField(memberName);
		return field.getType();
	}
	
	public Object castObjectToType(Object o, String type) throws Exception {
		Class<?> typeClass = Class.forName(type);
		o = typeClass.cast(o);
		return o;
	}
	
	public List<String> getGetterMethods(Class<?> from) throws Exception {
        List<String> result = new ArrayList<String>();
        BeanInfo info = Introspector.getBeanInfo(from);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            Method reader = pd.getReadMethod();
            if (reader != null) {
                String name = pd.getName();
                if (!"class".equals(name)) {
                    result.add(name);
                }
            }
        }
        return result;
    }
	
}
```
