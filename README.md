# Future.cs
very simple future implementation for .Net 3.5 or below, without promise

Source
----
```c#
public sealed class Future<T>
{
    object m = new object();
    T storage;
    bool hasValue = false;

    public void Produce(T value)
    {
        if (hasValue == true)
		throw new InvalidProgramException ();
				
        lock (m)
        {
            storage = value;
            hasValue = true;

            Monitor.Pulse(m);
        }
    }
    public T Acquire()
    {
        if(hasValue == false)
        {
            lock (m)
            {
                while (hasValue == false)
                {
                    Monitor.Wait(m);
                }
            }
        }

        return storage;
    }
};
```
