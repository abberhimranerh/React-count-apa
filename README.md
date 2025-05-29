import React, { useState, useEffect } from 'react';

// Reusable List Component
const ListComponent = ({ items, renderItem }) => {
  return (
    <ul style={{ listStyle: 'none', padding: 0 }}>
      {items.map((item, index) => (
        <li key={index} style={{ padding: '8px', borderBottom: '1px solid #eee' }}>
          {renderItem ? renderItem(item) : item.toString()}
        </li>
      ))}
    </ul>
  );
};

// Parent Component that fetches data
const ApiDataFetcher = () => {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        // Using JSONPlaceholder API as an example
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const jsonData = await response.json();
        setData(jsonData);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) {
    return <div>Loading data...</div>;
  }

  if (error) {
    return <div>Error: {error}</div>;
  }

  if (data.length === 0) {
    return <div>No data available</div>;
  }

  return (
    <div>
      <h2>User List</h2>
      <ListComponent
        items={data}
        renderItem={(user) => (
          <div>
            <strong>{user.name}</strong> - {user.email}
            <br />
            <small>{user.company.name}</small>
          </div>
        )}
      />
    </div>
  );
};

export default ApiDataFetcher;
