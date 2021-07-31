---
layout: post
title:      "Structural Design Pattern: Flyweight"
date:       2021-07-29 04:40:22 -0400
description: "...."
permalink:  design_pattern_flyweight
---

Flyweight design pattern uses sharing to support large numbers of fine-grained objects efficiently.

it can minimizes memory usage by sharing some of its data with other similar objects rather creating a new ones.

To reduce memory usage, you share objects that are similar in some way rather than creating new ones.

This pattern's effectiveness depends heavily on how and where it's used. Apply the Flyweight pattern when all of the following are true:
* An application uses a large number of objects.
* Storage costs are high because of the sheer quantity of objects.
* Most object state can be made extrinsic.
* Many groups of objects may be replaced by relatively few shared objects once extrinsic state is removed.
* The application doesn't depend on object identity. Since flyweight objects may be shared, identity tests will return true for conceptually distinct objects.

Code:

```
public class Point {
    private int x;  // 4 bytes
    private int y;  // 4 bytes
    private PointIcon icon;

    public Point(int x, int y, PointIcon icon) {
        this.x = x;
        this.y = y;
        this.icon = icon;
    }

    public void draw() {
        System.out.printf("%s at (%d, %d)", icon.getType(), x, y);
    }
}
```

```
point type
```
public enum PointType {
    HOSPITAL,
    CAFE,
    RESTAURANT
}
```
```

```
public class PointIcon {
    private final PointType type; // 4 bytes
    private final byte[] icon;    // 20 KB -> 20 MB

    public PointIcon(PointType type, byte[] icon) {
        this.type = type;
        this.icon = icon;
    }

    public PointType getType() {
        return type;
    }
}
```

```
public class PointService {
    private PointIconFactory iconFactory;

    public PointService(PointIconFactory iconFactory) {
        this.iconFactory = iconFactory;
    }

    public List<Point> getPoints() {
        List<Point> points = new ArrayList<>();
        Point point = new Point(1, 2, iconFactory.getPointIcon(PointType.CAFE));
        points.add(point);

        return points;
    }

}
```

```
public class PointIconFactory {
    private Map<PointType, PointIcon> icons = new HashMap<>();

    public PointIcon getPointIcon(PointType type) {
        // make connection between object and
        if (!icons.containsKey(type)) {
            PointIcon icon = new PointIcon(type, null);
            icons.put(type, icon);
        }

        return icons.get(type);
    }
}
```

```
public static void main(String[] args){
    PointService service = new PointService(new PointIconFactory());
    for(Point point : service.getPoints()){
        point.draw();
    }
}
```
```
//Output:
CAFE at (1, 2)
```

